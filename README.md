# TUGAS-AKHIR-PRAKTIKUM-PEMODELAN-OSEANOGRAFI-KELOMPOK-16
Repositori ini dibuat sebagai pemenuhan tugas akhir kelompok praktikum pemodelan oseanografi 2022. Repositori ini berisi 4 modul yang dipelajari selama praktikum pemodelan oseanografi yang terdiri dari script, hasil pemodelan dan pembahasan atau penjelasn hasilnya. pemrograman python yang dapat dilakukan pada beberapa platform seperti Google Colaboratory dan Jupyter Notebook. Sedangkan untuk library yang digunakan kali ini adalah Numpy, Matplotlib, Sys, 
# AUTHORS (KELOMPOK 16)

# MODUL 1

# MODUL 2 : ADVEKSI DIFUSI 2D
Proses penyebaran polutan terjadi melalui dua proses utama yaitu difusi dan adveksi, dan dapat dianggap dua mekanisme yang terpisah. Adveksi adalah proses perpindahan panas sebagai akibat dari adanya aliran. Difusi adaalah proses perpindahan panas berupa rambatan dari air dengan temperatur tinggi ke air dengan temperatur yang lebih rendah.

Dasar dalam membangun model 2D untuk transpor adveksi adalah persamaan matematis sebagai berikut.

![1 ad](https://user-images.githubusercontent.com/105967656/169827445-f5627e34-8a2e-4d83-821b-dbbb528bf7c7.png)

Sedangkan dalam membangun model 2D untuk transpor dengan mekanisme difusi, dibangun dari persamaan matematis sebagai berikut.

![2 ad](https://user-images.githubusercontent.com/105967656/169828458-50431384-0861-4ec0-be07-d24765c99f22.png)

Persamaan adveksi dan difusi di atas merupakan persamaan umum yang menggambarkan proses adveksi serta difusi yang terjadi pada suatu materi sehingga untuk membentuk suatu persamaan model 2D yang mendekati proses kejadian di alam maka perlu adanya diskritisasi terhadap persamaan tersebut.

Diskritisasi merupakan suatu metode untuk mencari solusi persamaan secara numerik dari suatu persamaan matematika sehingga dapat dinyatakan baik dalam dimensi ruang ataupun waktu. Proses diksritisasi model 2D pada suku adveksi umumnya menggunakan metode eksplisit upstream. Metode yang sama juga berlaku untuk deskritisasi suku difusi. Metode eksplisit upstream merupakan metode dimana persamaan beda hingga menggunakan pendekatan beda maju untuk turunan waktu, sedangkan untuk turunan terhadap ruang dilakukan dengan melihat arah kecepatan u. Jika u > 0 maka turunan terhadap ruang menggunakan pendekatan beda mundur, sebaliknya jika u < 0 digunakan pendekatan beda maju.

Persamaan dari metode diskritisasi untuk suku adveksi 2D adalah sebagai berikut.

![3 ad](https://user-images.githubusercontent.com/105967656/169830848-0cf35f66-21a0-46a0-9672-5d42187b6d0e.png)
![4 ad](https://user-images.githubusercontent.com/105967656/169830895-df8105d1-90eb-4b3c-bafe-9f6262aeccea.png)

Model 2D untuk mekanisme transpor difusi dapat menggunakan pendekatan beda maju untuk turunan waktu dan beda pusat untuk turunan ruang. Indeks n untuk waktu, indeks i untuk ruang, dan koefisiesn difusi AD dianggap konstan terhadap ruang dan waktu.

Persamaan diskritisasi untuk model 2D difusi adalah sebagai berikut.

![5 ad](https://user-images.githubusercontent.com/105967656/169831809-82eeb46b-68ce-4fd2-b28d-dcbd04b01fdb.png) ![6 ad](https://user-images.githubusercontent.com/105967656/169831852-cd31d095-6632-4244-b769-e32124d18886.png)

Persamaan diskritisasi adveksi dan difusi di atas jika digabungkan menjadi persamaan berikut.

![7 ad](https://user-images.githubusercontent.com/105967656/169833178-d69bb74c-367c-40a9-8c37-8e7220362b85.png)
![8 ad](https://user-images.githubusercontent.com/105967656/169833213-c0c79c64-f80b-4874-bc45-e761f7eb2766.png)

Penentuan Nilai Batas dan Syarat Batas

Syarat batas merupakan suatu kondisi yang menggambarkan kondisi di batas baik ruang maupun waktu dari model yang dibangun.

Syarat batas dari metode eksplisit upstream diberikan pada nilai awal (hulu) dan nilai akhir (hilir).



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

# MODUL 3
# MODUL 4
