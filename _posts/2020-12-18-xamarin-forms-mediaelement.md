---
layout: post
title:  "Xamarin.Forms Community Toolkit MediaElement "
author: bryan
categories: [ Xamarin.Forms, MediaElement ]
image: assets/images/001/cp1.png
tags: [featured]
---
En ocaciones necesitamos utilizar un control de reproducción de video o audio, por eso me parecio muy interesante que el nuevo Xamarin Forms Community Toolkit incluye el MediaElement, este control puede reproducir contenido desde :

- La web, mediante un URI (HTTP o HTTPS).
- Un recurso incrustado en la aplicación de la plataforma con el ms-appx:/// esquema del URI.
- Archivos que proceden de las carpetas de datos locales y temporales de la aplicación, mediante el ms-appdata:/// esquema de URI.
- Biblioteca del dispositivo.

### Probémoslo

Crearemos un proyecto que muestre un video de una URI para demostrar el uso de MediaElement.

<div align="center">
<a href="/assets/images/post1_p1.png" data-fancybox>
<img src="/assets/images/post1_p1.png" /></a>
</div>

Creemos un nuevo proyecto con Microsoft Visual Studio Community 2019 (usare la Versión 16.8.3).

De clic en “Create a new project“

<div align="center">
<a href="/assets/images/001/1.png" data-fancybox>
<img src="/assets/images/001/1.png" /></a>
</div>

En la caja de texto puedes escribir “Xamarin.Forms” para buscar la plantilla de proyecto, y seleccione “Mobile App(Xamarin.Forms)” y presione el botón Next.

<div align="center">
<a href="/assets/images/001/22.png" data-fancybox>
<img src="/assets/images/001/22.png" /></a>
</div>

En Project Name escriba XamarinFormsCT, en Location, seleccione una ruta corta como C:\Code\, presione el botón Create.

<div align="center">
<a href="/assets/images/0012B.png" data-fancybox>
<img src="/assets/images/001/2B.png" /></a>
</div>

En Template selecciones Blank, en Platform seleccione Android, iOS y de clic en el botón Create.

<div align="center">
<a href="/assets/images/001/3.png" data-fancybox>
<img src="/assets/images/001/3.png" /></a>
</div>

De clic derecho sobre la solución y de clic en Manage Nuget Packages for Solution…

<div align="center">
<a href="/assets/images/001/33.png" data-fancybox>
<img src="/assets/images/001/33.png" /></a>
</div>

En la Pestaña Installed, active el check de "Include Prerelease", seleccione Xamarin.Forms despliegue el listado de versiones disponibles y seleecione la versión 5.0.0.1791-pre5 y de clic en el botón Install.

<div align="center">
<a href="/assets/images/001/4.png" data-fancybox>
<img src="/assets/images/001/4.png" /></a>
</div>

El MediaElement aparecio desde la versión 4.5 de Xamarin Forms, pero necesitaba el Flag "MediaElement_Experimental", que se agrega al código del archivo app.Xaml.CS

```csharp
Device.SetFlags(new string[]{ "MediaElement_Experimental" });
```

Pero si usamos la version Prerelease de Xamarin.Forms 5, no es necesario agregar este este flag, por eso actualizamos el Nugget.

Seleccione la pestaña Browse, escriba el nombre Xamarin.CommunityToolkit, seleccione el Nugget con la versión 1.0.0-pre6, seleccione todos los proyectos y de clic en botón Install. 

<div align="center">
<a href="/assets/images/001/5.png" data-fancybox>
<img src="/assets/images/001/5.png" /></a>
</div>

Sobre el proyecto XamarinFormsCT, de clic derecho y seleccione "Add" y seleccione  "New folder" y escriba el nombre "Views" y presione enter.

<div align="center">
<a href="/assets/images/001/55.png" data-fancybox>
<img src="/assets/images/001/55.png" /></a>
</div>

Sobre el folder Views, de Clic Derecho "Add" y seleccione "New Item"

<div align="center">
<a href="/assets/images/001/6.png" data-fancybox>
<img src="/assets/images/001/6.png" /></a>
</div>

En la nueva ventana busque Content Page y nombrelo "MediaElementPage.xaml" y de clic en el botón "Add".

<div align="center">
<a href="/assets/images/001/7.png" data-fancybox>
<img src="/assets/images/001/7.png" /></a>
</div>

En el "MediaElementPage.xaml" remplace el código el siguiente:

```xml
<?xml version="1.0" encoding="utf-8" ?>
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:mctrl="http://xamarin.com/schemas/2020/toolkit"
             x:Class="XamarinFormsCT.Views.MediaElementPage">
    <Grid>
        <mctrl:MediaElement x:Name="media" 
                            Source="https://sec.ch9.ms/ch9/1bec/36d0a8ed-175d-4e1b-8ae6-f88df3fa1bec/XAMSHOWFormsFiveIsHERE_high.mp4" 
                            Aspect="AspectFill" ShowsPlaybackControls="True" />
    </Grid>
</ContentPage>
```
Puede observar este NameSpace
```xml
xmlns:mctrl="http://xamarin.com/schemas/2020/toolkit"
```
Este es el que nos permitira utilizar el Xamarin.Communitytoolkit, con el cual se puede utilizar el MediaElement, para repoducir elementos multimedia.

Abra el archivo "MediaElementPage.xaml.cs" y pegue este código

```csharp
using System;

namespace XamarinFormsCT.Views
{

    public partial class MediaElementPage 
    {
        public MediaElementPage()
        {
            InitializeComponent();
        }
    }
}
```
Ahora habra el archivo App.Xaml.cs y remplace el contenido con este código

```csharp
using System;
using Xamarin.Forms;
using Xamarin.Forms.Xaml;
using XamarinFormsCT.Views;

namespace XamarinFormsCT
{
    public partial class App : Application
    {
        public App()
        {
            InitializeComponent();

            MainPage = new MediaElementPage();
        }

    }
}
```

El código fuente puedes encontrarlo aquí:

<div align="center">
<img src="/assets/images/source__code.png" />
</div>
[https://github.com/BryanOroxon/XamarinFormsCT](https://github.com/BryanOroxon/XamarinFormsCT)


El resultado del este proyecto debe verse así, con todos los pasos que hemos realizado este proyecto, funciona tanto en Android como en iOS.

<div align="center">
<a href="/assets/images/001/cp2.png" data-fancybox>
<img src="/assets/images/001/cp2.png" /></a>
</div>

<div align="center">
<a href="/assets/images/001/cp3.png" data-fancybox>
<img src="/assets/images/001/cp3.png" /></a>
</div>


## Conclusión

MediaElement nos permite crear App's capaces de reproducir audio y video de una serie de recursos diferentes muy fácilmente y con poco código, ahora te invito a que lo pruebes.

Este Post es parte del segundo calendario de Adviento de la Comunidad Xamarin en Español, para más información sigue el link.

[https://www.luisbeltran.mx/2020/11/16/segundo-calendario-de-adviento-de-xamarin-en-espanol/](https://www.luisbeltran.mx/2020/11/16/segundo-calendario-de-adviento-de-xamarin-en-espanol/)


Muchas Gracias por darte el tiempo de leer este post.

P.D. un gran agradecimiento a mi amigo Daniel Monettelli por su ayuda en la creación de mi Github Page.