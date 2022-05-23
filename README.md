# TUGAS-AKHIR-PRAKTIKUM-PEMODELAN-OSEANOGRAFI-KELOMPOK-16
Repositori ini dibuat sebagai pemenuhan tugas akhir kelompok praktikum pemodelan oseanografi 2022. Repositori ini berisi 4 modul yang dipelajari selama praktikum pemodelan oseanografi yang terdiri dari script, hasil pemodelan dan pembahasan atau penjelasn hasilnya. pemrograman python yang dapat dilakukan pada beberapa platform seperti Google Colaboratory dan Jupyter Notebook. Sedangkan untuk library yang digunakan kali ini adalah Numpy, Matplotlib, Sys, 
# AUTHORS (KELOMPOK 16)

# MODUL 1

# MODUL 2 : ADVEKSI DIFUSI 2D
    import matplotlib.pyplot as plt
    import numpy as np
    import sys

    def percentage(part, whole):
        percentage = 100 * float(part)/float(whole)
        return str(round(percentage,2)) + "x"

    #Masukan Parameter Awal
    C = 1.90
    ad = 1.90

    #arah arus
    theta = 90
    #theta = 150
    #theta = 225
    #theta = 405

    #paramter lanjutan
    q = 0.95 #kriteria kestabilan
    x = 500 #jumlah grid horizontal
    y = 500 #jumlah grid vertikal
    dx = 5
    dy = 5

    #lama simulasi
    Tend = 109
    dt = 0.5

    #Polutan
    px = 250 #polutan pada grid x
    py = 239 #polutan pada grid y
    Ic = 1090 #jumah polutan

    #Perhitungan U dan V
    u = C * np.sin(theta*np.pi/180)
    v = C * np.cos(theta*np.pi/180)
    dt_count = 1/((abs(u)/(q*dx))+(abs(v)/(q*dy))+(2*ad/(q*dx**2))+(2*ad/(q*dy*2)))

    Nx = int(x/dx)  #number of mesh in x direction
    Ny = int(y/dy)  #number of mesh in y direction
    Nt = int(Tend/dt) #number of timestep

    #perhitungan titik polutan di buang
    px1 = int(px/dx)
    py1 = int(py/dy)

    #fungsi disederhanakan
    lx = u*dt/dx
    ly = v*dt/dy
    ax = ad*dt/dx**2
    ay = ad*dt/dy**2
    cfl = (2*ax + 2*ay + abs(lx) + abs(ly))  #syarat kestabilan CFL

    #perhitungan cfl
    if cfl >= q:
        print('CFL Violated, please use dt :'+str(round(dt_count,4)))
        sys.exit ()

    #pembuatan grid 
    x_grid = np.linspace(0-dx, x+dx, Nx+2) #ghostnode boundary
    y_grid = np.linspace(0-dx, y+dy, Ny+2) #ghostnode boundary
    t = np.linspace(0, Tend, Nt+1)
    x_mesh, y_mesh = np.meshgrid(x_grid,y_grid)
    F = np.zeros((Nt+1, Ny+2, Nx+2))

    #kondisi awal
    F[0,py1,px1]=Ic

    #Iterasi
    for n in range (0, Nt):
        for i in range (1,Ny+1):
               for j in range (1, Nx+1):
                F[n+1,i,j]=((F[n,i,j]*(1-abs(lx)-abs(ly))) + \
                       (0.5*(F[n,i-1,j]*(ly+abs(ly)))) + \
                       (0.5*(F[n,i+1,j]*(abs(ly)-ly))) + \
                       (0.5*(F[n,i,j-1]*(lx+abs(lx)))) + \
                       (0.5*(F[n,i,j+1]*(abs(lx)-lx))) + \
                       (ay*(F[n,i+1,j]-2*(F[n,i,j])+F[n,i-1,j])) +\
                       (ax*(F[n,i,j+1]-2*(F[n,i,j])+F[n,i,j-1])))
        #syarat batas (boundary condition)
        F[n+1,0,:] = 0 #bc bawah
        F[n+1,:,0] = 0 #bc kiri
        F[n+1,Ny+1,:] = 0 #bc atas
        F[n+1,:,Nx+1] = 0 #bc kanan
    
    #Output Gambar
    plt.clf()
    plt.pcolor(x_mesh, y_mesh, F[n+1, :, :], cmap = 'jet',shading='auto',edgecolor='k')
    cbar=plt.colorbar(orientation='vertical',shrink=0.95,extend='both')
    cbar.set_label(label='Concentration',size = 8)
    #plt.clim(0,100)
    plt.title('NAMA_NIM \n t='+str(round(dt*(n+1),3))+ ', Initial condition='+str(Ic),fontsize=10)
    plt.xlabel('x_grid',fontsize=9)
    plt.ylabel('y_grid',fontsize=9)
    plt.axis([0, x, 0, y])
    #plt.pause(0.01)
    plt.savefig(str(n+1)+'.jpg', dpi=300)
    plt.pause(0.01)
    plt.close()
    print('running timestep ke:' +str(n+1) + ' dari:' +str(Nt) + '('+ percentage(n+1,Nt)+')')
    print('Nilai CFL:' +str(cfl) + ' dengan arah: ' +str(theta))

# MODUL 3 : MODEL HIDRODINAMIKA 1D
Model Hidrodinamika merupakan suatu model yang dibangun dari adanya proses-proses yang mempengaruhi pergerakan massa air misalnya simulasi elevasi muka air laut dan arus yang dipengaruhi oleh beberapa parameter. Pemodelan ini melibatkan konversi massa atau kontinuitas dan hukum momentum dalam perhitungannya.

Model hidrodinamika ini memerlukan banyak data untuk mensimulasikan suatu model. Selain itu proses simulasi yang lama karena _timestep_ yang digunakan cenderung kecil sehingga proses _running_ lebih lama dan juga rawan eror ketika perhitungan aliran kritis.

## Persamaan yang Digunakan
1. Persamaan Momentum

![image](https://user-images.githubusercontent.com/105999278/169819353-d3588ab7-8f95-4d13-b578-d5c13d873160.png)

2. Persamaan Kontinuitas

![image](https://user-images.githubusercontent.com/105999278/169819422-dc7f6c54-55f5-4e28-9f70-192d5f99ee62.png)

3. Persamaan Pembangun

![image](https://user-images.githubusercontent.com/105999278/169819686-91d2f1fe-3f0c-43d9-9a16-c3452fc128c6.png)

4. Persamaan Transport

![image](https://user-images.githubusercontent.com/105999278/169819789-df8015f0-a638-43c5-ae69-b8c61ef41414.png)

## Diskretisasi


## Penyelesaian Analitik
![image](https://user-images.githubusercontent.com/105999278/169820467-2ee3c3de-05f3-41b2-8c7d-54810c7a1a3f.png)

Keterangan:

H	: Kedalaman terukur, konstan terhadap ruang (m)

A	: Amplitudo

U	: Kecepatan sesaat (m/s)

ζ	: Elevasi (m)



# MODUL 4
