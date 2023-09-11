# ERA-5_Reanalisys_Models_Interested_Zones

from netCDF4 import Dataset
import numpy as np
import matplotlib.pyplot as plt
from mpl_toolkits.basemap import Basemap
import PIL



lo=-28.9
la=12.2


x0 = ([-94.5, -94.3, -94.1, -93.3, -92.2, -91.7, -91.9, -92.3, -92.9, -93.1, -93.9, -94.5])
y0 = ([18.3, 17.7, 17.3, 17.3, 17.7, 19.0, 19.5, 20.0, 19.5, 18.8, 18.6, 18.3])

x85 = ([-95.3, -95.2, -94.8, -93.3, -91.6, -91.0, -91.1, -92.3, -93.7, -93.8, -94.6, -95.3])
y85 = ([18.5, 17.6, 17.0, 16.6, 17.4, 19.1, 19.6, 20.8, 19.8, 19.3, 19.0, 18.5])

x150 = ([-95.9, -95.8, -95.4, -93.2, -91.1, -90.4, -90.5, -92.2, -94.2, -94.4, -95.1, -95.9])
y150 = ([18.8, 17.6, 16.7, 16.0, 17.0, 19.3, 20.0, 21.4, 20.2, 19.6, 19.4, 18.8])

x300 = ([-97.1, -97.2, -96.6, -92.8, -89.9, -89.0, -89.2, -91.8, -95.4, -95.6, -96.3, -97.1])
y300 = ([19.5, 17.5, 16.0, 14.7, 16.3, 19., 20.4, 22.7, 21.2, 20.3, 20.1, 19.5])

xns = (-93.0, -93.0)
yns = (22.2, 14.7)

xwe = (-97.2, -89.2)
ywe = (18.6, 18.6)



ct_lat = ([12.2,12.1,12,11.9,11.8,11.8,11.8,11.9,12,12.3,12.8,13,13.2,13.5,13.8,14,14.2,14.4,14.8,14.9,15,15.4,15.9,16.1,16.4,16.8,17.1,17.5,17.6,17.8,18,18.2,18.6,18.7,18.9,19.2,19.7,20.1,20.5,20.5,20.5,20.5])
# =============================================================================# =============================================================================
ct_lon = ([-28.9,-30.7,-32.4,-34.5,-36.5,-38.3,-40.1,-41.7,-43.4,-45.1,-47,-49.2,-51.3,-53.3,-55.5,-57.7,-59.8,-61.7,-63.5,-65.1,-66.6,-68,-69.5,-71,-72.6,-74.3,-76,-77.8,-79.8,-81.5,-83.3,-85.1,-86.9,-87.7,-88.7,-90.5,-92.2,-94,-95.5,-97,-97.3,-99])



days = np.arange(0,10)

for a in days:
    
    b=-100
    c=6
    d=-19.2
    e=24
    
    data = Dataset(r'D:\Datos_ERA5\Dean2007\DEAN_TARYECTORIA.nc')

    lats = data.variables['latitude'][:]
    lons = data.variables['longitude'][:]
    time = data.variables['time'][:]
    temperatura = data.variables['sst'][:]
    velocidad = data.variables['v10'][:]
    presion = data.variables['msl'][:]
    altura_ola = data.variables['swh'][:]
    precipitacion = data.variables['tp'][:]
    cape = data.variables['cape'][:]
    velocidadu = data.variables['u10'][:]

    	
    temp_c = temperatura - 273.15
    presion_hpa = presion * 0.01
    precipitacion_mm_hr = precipitacion * 1000


    #Creando el multimapa
    fig = plt.figure(figsize=(20, 10))


    #creando los mapas

    #Temperatura de la Superficie del mar 

    ax = fig.add_subplot(3,4,5)
    ax.set_title('Temperatura de la superficie del mar')

    mp = Basemap(ax = ax,
                 projection = 'merc',
                 llcrnrlon = b,
                 llcrnrlat = c,
                 urcrnrlon = d,
                 urcrnrlat = e,
                 resolution = 'i')

    lon, lat = np.meshgrid(lons, lats)
    x,y = mp(lon, lat)

    temp = mp.pcolor(x, y, np.squeeze(temp_c[a , :, :]), cmap='jet')
    mp.drawcoastlines()
    mp.drawstates()
    mp.drawcountries()
    cbar = mp.colorbar(temp, location = 'right',  label='ºC')

    a0, b0 = mp(x0, y0)
    plt.plot(a0, b0)
    a85, b85 = mp(x85, y85)
    plt.plot(a85, b85)
    a150, b150 = mp(x150, y150)
    plt.plot(a150, b150)
    a300, b300 = mp(x300, y300)
    plt.plot(a300, b300)
    uu, bb = mp(lo,la) 
    plt.scatter(uu, bb, marker='.', color='Red')

        

    #Componente V del Viento a 10 m
    ax = fig.add_subplot(3,4,3)
    ax.set_title('Componente -V- del viento a 10 m')

    mp = Basemap(ax = ax,
                 projection = 'merc',
                 llcrnrlon = b,
                 llcrnrlat = c,
                 urcrnrlon = d,
                 urcrnrlat = e,
                 resolution = 'i')

    lon, lat = np.meshgrid(lons, lats)
    x,y = mp(lon, lat)



    vient = mp.pcolor(x, y, np.squeeze(velocidad[a , :, :]), cmap='nipy_spectral')
    mp.drawcoastlines()
    mp.drawstates()
    mp.drawcountries()
    cbar = mp.colorbar(vient, location = 'right', label='m / s')

    a0, b0 = mp(x0, y0)
    plt.plot(a0, b0)
    a85, b85 = mp(x85, y85)
    plt.plot(a85, b85)
    a150, b150 = mp(x150, y150)
    plt.plot(a150, b150)
    a300, b300 = mp(x300, y300)
    plt.plot(a300, b300)
    uu, bb = mp(lo,la) 
    plt.scatter(uu, bb, marker='.', color='Red')




    #Componente U del Viento a 10 m
    ax = fig.add_subplot(3,4,4)
    ax.set_title('Componente -U- del viento a 10 m')

    mp = Basemap(ax = ax,
                 projection = 'merc',
                 llcrnrlon = b,
                 llcrnrlat = c,
                 urcrnrlon = d,
                 urcrnrlat = e,
                 resolution = 'i')

    lon, lat = np.meshgrid(lons, lats)
    x,y = mp(lon, lat)



    vientU = mp.pcolor(x, y, np.squeeze(velocidadu[a , :, :]), cmap='nipy_spectral')
    mp.drawcoastlines()
    mp.drawstates()
    mp.drawcountries()
    cbar = mp.colorbar(vientU, location = 'right', label='m / s')

    a0, b0 = mp(x0, y0)
    plt.plot(a0, b0)
    a85, b85 = mp(x85, y85)
    plt.plot(a85, b85)
    a150, b150 = mp(x150, y150)
    plt.plot(a150, b150)
    a300, b300 = mp(x300, y300)
    plt.plot(a300, b300)
    uu, bb = mp(lo,la) 
    plt.scatter(uu, bb, marker='.', color='Red')


    # Presion al nivel medio del mar
    ax = fig.add_subplot(3,4,7)
    ax.set_title('Presión al nivel medio del mar')

    mp = Basemap(ax = ax,
                 projection = 'merc',
                 llcrnrlon = b,
                 llcrnrlat = c,
                 urcrnrlon = d,
                 urcrnrlat = e,
                 resolution = 'i')

    lon, lat = np.meshgrid(lons, lats)
    x,y = mp(lon, lat)


    prenmm = mp.pcolor(x, y, np.squeeze(presion_hpa[a , :, :]), cmap='rainbow')
    mp.drawcoastlines()
    mp.drawstates()
    mp.drawcountries()
    cbar = mp.colorbar(prenmm, location = 'right', label='hPa')

    a0, b0 = mp(x0, y0)
    plt.plot(a0, b0)
    a85, b85 = mp(x85, y85)
    plt.plot(a85, b85)
    a150, b150 = mp(x150, y150)
    plt.plot(a150, b150)
    a300, b300 = mp(x300, y300)
    plt.plot(a300, b300)
    uu, bb = mp(lo,la) 
    plt.scatter(uu, bb, marker='.', color='Red')


    #Altura significativa de las olas combinadas con el viento y oleaje
    data2= Dataset(r'D:\Datos_ERA5\Dean2007\DEAN_TRAYECTORIA_OLAS.nc' )

    lats2= data2.variables['latitude'][:]
    lons2= data2.variables['longitude'][:]
    time2= data2.variables['time'][:]
    altura_ola = data2.variables['swh'][:]


    ax = fig.add_subplot(3,4,8)
    ax.set_title('Altura significativa de las olas')

    mp2= Basemap(projection = 'merc',
                 llcrnrlon = b,
                 llcrnrlat = c,
                 urcrnrlon = d,
                 urcrnrlat = e,
                 resolution = 'i')

    lon2, lat2= np.meshgrid(lons2, lats2)
    x2,y2= mp2(lon2, lat2)

    altol=mp2.pcolor(x2, y2, np.squeeze(altura_ola[a , :, :]), cmap='turbo')
    mp2.drawcoastlines()
    mp2.drawstates()
    mp2.drawcountries()
    cbar =mp2.colorbar(altol, location = 'right',  label='m')

    a0, b0 = mp(x0, y0)
    plt.plot(a0, b0)
    a85, b85 = mp(x85, y85)
    plt.plot(a85, b85)
    a150, b150 = mp(x150, y150)
    plt.plot(a150, b150)
    a300, b300 = mp(x300, y300)
    plt.plot(a300, b300)
    uu, bb = mp(lo,la) 
    plt.scatter(uu, bb, marker='.', color='Red')


    #PrecipitaciÓn 
    ax = fig.add_subplot(3,4,6)
    ax.set_title('Precipitación')

    mp = Basemap(ax = ax,
                 projection = 'merc',
                 llcrnrlon = b,
                 llcrnrlat = c,
                 urcrnrlon = d,
                 urcrnrlat = e,
                 resolution = 'i')

    lon, lat = np.meshgrid(lons, lats)
    x,y = mp(lon, lat)



    preci = mp.pcolor(x, y, np.squeeze(precipitacion_mm_hr[a , :, :]), cmap='jet')
    mp.drawcoastlines()
    mp.drawstates()
    mp.drawcountries()
    cbar = mp.colorbar(preci, location = 'right', label='mm / hr')

    a0, b0 = mp(x0, y0)
    plt.plot(a0, b0)
    a85, b85 = mp(x85, y85)
    plt.plot(a85, b85)
    a150, b150 = mp(x150, y150)
    plt.plot(a150, b150)
    a300, b300 = mp(x300, y300)
    plt.plot(a300, b300)
    uu, bb = mp(lo,la) 
    plt.scatter(uu, bb, marker='.', color='Red')


    # Energía potencial convectiva disponible (CAPE)
    ax = fig.add_subplot(3,4,2)
    ax.set_title('(CAPE)')

    mp = Basemap(ax = ax,
                 projection = 'merc',
                 llcrnrlon = b,
                 llcrnrlat = c,
                 urcrnrlon = d,
                 urcrnrlat = e,
                 resolution = 'i')

    lon, lat = np.meshgrid(lons, lats)
    x,y = mp(lon, lat)



    enercape = mp.pcolor(x, y, np.squeeze(cape[a , :, :]), cmap='jet')
    mp.drawcoastlines()
    mp.drawstates()
    mp.drawcountries()
    cbar = mp.colorbar(enercape, location = 'right', label='J / kg')

    a0, b0 = mp(x0, y0)
    plt.plot(a0, b0)
    a85, b85 = mp(x85, y85)
    plt.plot(a85, b85)
    a150, b150 = mp(x150, y150)
    plt.plot(a150, b150)
    a300, b300 = mp(x300, y300)
    plt.plot(a300, b300)
    uu, bb = mp(lo,la) 
    plt.scatter(uu, bb, marker='.', color='Red')



    # UBICACION
    ax = fig.add_subplot(3,4,1)
    ax.set_title('Centro del CT')

    mp = Basemap(ax = ax,
                 projection = 'merc',
                 llcrnrlon = b,
                 llcrnrlat = c,
                 urcrnrlon = d,
                 urcrnrlat = e,
                 resolution = 'i')
    lon, lat = np.meshgrid(lons, lats)
    x,y = mp(lon, lat)

    mp.drawcoastlines()
    mp.drawstates()
    mp.drawcountries()
    mp.bluemarble(scale=0.2)

    tray, ectoria = mp(ct_lon, ct_lat)
    plt.scatter(tray, ectoria, marker='o', color='white')


    u, b = mp(lo, la)
    plt.scatter(u, b, marker='o', color='Red')

    a0, b0 = mp(x0, y0)
    plt.plot(a0, b0)
    a85, b85 = mp(x85, y85)
    plt.plot(a85, b85)
    a150, b150 = mp(x150, y150)
    plt.plot(a150, b150)
    a300, b300 = mp(x300, y300)
    plt.plot(a300, b300)
    plt.plot(tray, ectoria, color='gray')

    day=a+1

    plt.suptitle('Dean 2007 13-23 de agosto (HU 5)', fontsize=22)
    #plt.tight_layout()
    #plt.subplots_adjust(right=0.833, left=0.2e)
    plt.savefig(str(day)+'.jpg')



#Creating the GIF    
image_frames = []

days = np.arange(1,9)

for k in days:
    new_frame = PIL.Image.open(r'D:/Datos_ERA5/GIF' + '\\' + str(k) + '.jpg')
    image_frames.append(new_frame)

image_frames[0].save('Tropical_ciclone_Dean_2007.gif', format = 'GIF',
                     append_images = image_frames[1: ],
                     save_all = True, duration = 1200,
                     loop = 0)

