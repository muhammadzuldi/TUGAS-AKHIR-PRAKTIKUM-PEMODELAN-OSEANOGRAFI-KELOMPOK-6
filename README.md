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

## Diskrtisasi
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

## Penentuan Nilai Batas dan Syarat Batas

Syarat batas merupakan suatu kondisi yang menggambarkan kondisi di batas baik ruang maupun waktu dari model yang dibangun.

Syarat batas dari metode eksplisit upstream diberikan pada nilai awal (hulu) dan nilai akhir (hilir).

![9 ad](https://user-images.githubusercontent.com/105967656/169835878-0370d29d-1ba3-4fcb-883d-07b348a121bd.png)

## Kriteria Kestabilan

Suatu metode untuk menentukan seberapa besar nilai stabilitas dari model yang dibangun.

Kriteria kestabilan yang digunakan untuk menyelesaikan pemodelan 2D adveksi difusi ini adalah sebagai berikut.

![10 ad](https://user-images.githubusercontent.com/105967656/169835595-b55cf6ca-5c38-4485-8c48-8f7cc28598da.png)

## Pengaplikasian Aveksi-Difusi dalam Bidang Oseanografi:

1.

2.

3.

## Script Python untuk Pemodelan

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

Model Hidrodinamika merupakan suatu model yang dibangun dari adanya proses-proses yang mempengaruhi pergerakan massa air misalnya simulasi elevasi muka air laut dan arus yang dipengaruhi oleh beberapa parameter dengan melibatkan konversi massa atau kontinuitas dan hukum momentum dalam perhitungannya. Pemodelan ini memerlukan waktu yang cukup lama dikarenakan _timestep_ yang digunakan harus kecil sehingga proses _running_ model akan lama.

## Pengaplikasian Model Hidrodinamika 
1.
2.
3.

## Persamaan yang Digunakan
**1. Persamaan Momentum**

![image](https://user-images.githubusercontent.com/105999278/169938943-3338e34e-1f22-48e5-95dc-d1ad5cc505c8.png)


**2. Persamaan Kontinuitas**

![image](https://user-images.githubusercontent.com/105999278/169938998-2a94f679-f3a8-4eae-8465-0acf116281e6.png)

**3. Persamaan Pembangun**

![image](https://user-images.githubusercontent.com/105999278/169939023-fc75eb60-aad0-4955-9f82-8c7d55d752f8.png)

**5. Persamaan Transport**

Dari persamaan pembangun diatas diubah menjadi persamaan transport sehingga persamaannnya adalah sebagai berikut.

![image](https://user-images.githubusercontent.com/105999278/169939741-305533a3-a442-4058-b00a-23658bbeb552.png)

Persamaan diatas ditinjau pada perubahan komponen Ut terhadap perubahan waktu       dan perubahan komponen ζ atau elevasi terhadap perubahan ruang.

![image](https://user-images.githubusercontent.com/105999278/169939805-cb942204-42ad-4437-b106-846b7f283bc8.png)

Persamaan diatas ditinjau pada perubahan komponen ζ atau elevasi terhadap           perubahan waktu dan perubahan komponen Ut terhadap perubahan ruang.

Dengan nilai

Ut = U x H
    
## Diskritisasi

Diskritisasi merupakan suatu proses kuantitasi sifat-sifat kontinu. Kegunaan diskritisasi adalah untuk mereduksi dan menyederhanakan data sehingga didapatkan data diskrit yang lebih mudah dipahami, digunakan, dan dijelaskan. Salah satu metode yang dapat memperkirakan bentuk diferensial kontinu menjadi bentuk diskrit ialah dengan metode beda hingga. Berikut adalah bentuk diskrit berdasarkan persamaan transport diatas.

![image](https://user-images.githubusercontent.com/105999278/169938326-1d32bd0e-7a58-4b0e-87d3-a64d4cfe1683.png)

**Syarat Batas**

![image](https://user-images.githubusercontent.com/105999278/169938406-8478f127-d424-4a41-8e97-7a519200e399.png)

## Penyelesaian Analitik

![image](https://user-images.githubusercontent.com/105999278/169939977-d7a9ff4f-a7dc-4724-81d5-2e6c01de2603.png)

Keterangan:

H   : Kedalaman terukur, konstan terhadap ruang (m)

A   : Amplitudo

U   : Kecepatan sesaat (m/s)

ζ   : Elevasi (m)

## Script Python untuk Pemodelan

    import matplotlib.pyplot as plt
    import numpy as np

    #PERHITUNGAN AWAL

    p = 5000      #Panjang Grid
    T = 1200      #Waktu Simulasi
    A = 0.5       #Amplitudo
    D = 15        #Depth/Kedalaman
    dt = 2
    dx = 100
    To = 300      #Periode

    g = 9.8
    pi = np.pi
    C = np.sqrt(g*D)   #Kecepatan Arus
    s = 2*pi/To        #Kecepatan Sudut Gelombang
    L = C*To           #Panjang Gelombang 
    k = 2*pi/L         #Koefisien Panjang Gelombang
    Mmax = int(p//dx)
    Nmax = int(T//dt)

    zo = [None for _ in range(Mmax)]
    uo = [None for _ in range(Mmax)]

    hasilu = [None for _ in range(Nmax)]
    hasilz = [None for _ in range(Nmax)]

    for i in range (1, Mmax+1):
        zo[i-1] = A*np.cos(k*(i)*dx)
        uo[i-1] = A*C*np.cos(k*((i)*dx+(0.5)*dx))/(D+zo[i-1])
    for i in range (1, Nmax+1):
        zb = [None for _ in range(Mmax)]
        ub = [None for _ in range(Mmax)]
        zb[0] = A*np.cos(s*(i)*dt)
        ub[-1] = A*C*np.cos(k*L-s*(i)*dt)/(D+zo[-1])
        for j in range (1, Mmax):
            ub[j-1] = uo[j-1]-g*(dt/dx)*(zo[j]-zo[j-1])
        for k in range (2, Mmax+1):
            zb[k-1] = zo[k-1]-(D+zo[k-1])*(dt/dx)*(ub[k-1]-ub[k-2])
            hasilu[i-1] = ub
            hasilz[i-1] = zb
        for p in range (0, Mmax):
            uo[p] = ub[p]
            zo[p] = zb[p]

    #PEMBUATAN GRAFIK

    def rand_col_hex_string():
        return f'#{format(np.random.randint(0,16777215), "#08x")[2:]}'

    hasilu_np = np.array(hasilu)
    hasilz_np = np.array(hasilz)

    fig0, ax0 = plt.subplots(figsize=(12,8))
    for i in range (1, 16):
        col0 = rand_col_hex_string()
        line, = ax0.plot(hasilu_np[:,i-1], c=col0, label=f'n={i}')
        ax0.legend()

        ax0.set(xlabel='Waktu', ylabel='Kecepatan Arus',
                title='''Nama_NIM
                Perubahan Kecepatan Arus dalam Grid Tertentu di Sepanjang Waktu''')
        ax0.grid()

    fig1, ax1 = plt.subplots(figsize=(12,8))
    for i in range (1, 16):
        col1 = rand_col_hex_string()
        line, = ax1.plot(hasilz_np[:,i-1], c=col1, label=f'n={i}')
        ax0.legend()

        ax1.set(xlabel='Waktu', ylabel='Elevasi Muka Air',
                title='''Nama_NIM
                Perubahan Elevasi Permukaan Air dalam Grid Tertentu di Sepanjang Waktu''')
        ax1.grid()

    fig2, ax2 = plt.subplots(figsize=(12,8))
    for i in range (1, 16):
        col2 = rand_col_hex_string()
        line, = ax2.plot(hasilu_np[i-1], c=col2, label=f't={i}')
        ax2.legend()

        ax2.set(xlabel='Grid', ylabel='Kecepatan Arus',
                title='''Nama_NIM
                Perubahan Kecepatan Arus dalam Waktu Tertentu di Sepanjang Grid''')
        ax2.grid()

    fig3, ax3 = plt.subplots(figsize=(12,8))
    for i in range (1, 16):
        col3 = rand_col_hex_string()
        line, = ax3.plot(hasilz_np[i-1], c=col3, label=f't={i}')
        ax3.legend()

        ax3.set(xlabel='Grid', ylabel='Elevasi Muka Air',
                title='''Nama_NIM
                Perubahan Elevasi Muka Air dalam Waktu Tertentu di Sepanjang Grid''')
        ax2.grid()

    plt.show()

### _Output_ 

![image](https://user-images.githubusercontent.com/105999278/169942379-be43cd20-a499-4d69-893a-2fad9193e52b.png)
![image](https://user-images.githubusercontent.com/105999278/169942399-013af694-fbfc-4f26-8afa-b470176bf141.png)
![image](https://user-images.githubusercontent.com/105999278/169942421-16b7c95f-f9bc-487b-a121-d760da5a5166.png)


# MODUL 4
