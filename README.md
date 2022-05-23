# TUGAS-AKHIR-PRAKTIKUM-PEMODELAN-OSEANOGRAFI-KELOMPOK-16
Repositori ini dibuat sebagai pemenuhan tugas akhir kelompok praktikum pemodelan oseanografi 2022. Repositori ini berisi 4 modul yang dipelajari selama praktikum pemodelan oseanografi yang terdiri dari script, hasil pemodelan dan pembahasan atau penjelasn hasilnya. pemrograman python yang dapat dilakukan pada beberapa platform seperti Google Colaboratory dan Jupyter Notebook. Sedangkan untuk library yang digunakan kali ini adalah Numpy, Matplotlib, Sys, 
# AUTHORS (KELOMPOK 16)

# MODUL 1

# MODUL 2 : ADVEKSI DIFUSI 2D

import matplotlib.pyplot as plt
import numpy as np
import sys

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
