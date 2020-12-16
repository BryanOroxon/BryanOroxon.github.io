---
layout: post
title:  "Gifs en Xamarin.Forms"
author: bryan
categories: [ Xamarin.Forms, GIF ]
image: assets/images/gifs_en_xamarin_forms.png
tags: [featured]
---
Ahora con **Xamarin.Forms** versión 4.4 podremos utilizar el soporte de archivos .gif y animar nuestras UI´s permitiendo agregar animaciones en Xamarin.forms tanto para Android, iOS y UWP, lo que cual es grandioso al darles vida a nuestras aplicaciones móviles.

<div align="center">
<a href="/assets/images/post1_p0.png" data-fancybox>
<img src="/assets/images/post1_p0.png" /></a>
</div>

El control Image ha sido extendido con la propiedad IsAnimationPlaying de esta forma en nuestro código XAML:

Es importante mencionar que Xamarin.Forms incluye la habilidad de descargar archivos, pero no soporta Caching o Streaming de Gif’s animados, esperemos que en alguna actualización futura incluyan estas nuevas características.

Recuerda que por definición, cuando un GIF animado es cargado, no se reproduce automáticamente, porque su valor por defecto es “Falso”, la animación debe activarse con el valor Booleano IsAnimationPlaying=“True”.

## ¿Qué necesitamos?

- Xamarin.Forms 4.4 o superior.
- En Xamarin.Android que este activado el soporte de Fast Renders, las animaciones no funcionarán si se usa Legacy Renders.
- En Xamarin.UWP se requiere una versión mínima de Windows 10 Anniversary Update (version 1607).

### Probémoslo

Crearemos un proyecto que pruebe una página estática, que navegue a una página con un CollectionView y otra página que use un CarouselView que carguen varios Gifs para probar como funcionan.

<div align="center">
<a href="/assets/images/post1_p1.png" data-fancybox>
<img src="/assets/images/post1_p1.png" /></a>
</div>

Creemos un nuevo proyecto con Microsoft Visual Studio Community 2019 (usare la Versión 16.4.1).

Da clic en “Create a new project“

<div align="center">
<a href="/assets/images/post1_p2.png" data-fancybox>
<img src="/assets/images/post1_p2.png" /></a>
</div>

En la caja de texto puedes escribir “Xamarin.forms” para buscar la plantilla de proyecto, y selecciona “Mobile App(Xamarin.Forms)” y presione el botón Next.


<div align="center">
<a href="/assets/images/post1_p3.png" data-fancybox>
<img src="/assets/images/post1_p3.png" /></a>
</div>

En Project Name escriba GifInMotion, en Location, selecciones una ruta corta como C:\Desa\, presione el botón Create.

<div align="center">
<a href="/assets/images/post1_p4.png" data-fancybox>
<img src="/assets/images/post1_p4.png" /></a>
</div>

En Template selecciones Blank, en Platform seleccione Android, iOS, Windows(UWP) y de clic en OK.

<div align="center">
<a href="/assets/images/post1_p5.png" data-fancybox>
<img src="/assets/images/post1_p5.png" /></a>
</div>

Se mostrara VS 2019 con las plataformas que seleccionó.

<div align="center">
<a href="/assets/images/post1_p6.png" data-fancybox>
<img src="/assets/images/post1_p6.png" /></a>
</div>

De clic derecho sobre la solución y de clic en Manage Nuget Packages for Solution…

<div align="center">
<a href="/assets/images/post1_p7.png" data-fancybox>
<img src="/assets/images/post1_p7.png" /></a>
</div>

En la siguiente ventana, de clic en la Pestaña Update, se mostrarán los Nugets que pueden actualizarse, de clic en Select all packages, y luego clic en el botón Update.

<div align="center">
<a href="/assets/images/post1_p8.png" data-fancybox>
<img src="/assets/images/post1_p8.png" /></a>
</div>

Es posible que se le muestre la siguiente ventana, para aceptar los términos de licencia, de clic en “I Accept”.

<div align="center">
<a href="/assets/images/post1_p9.png" data-fancybox>
<img src="/assets/images/post1_p9.png" /></a>
</div>

Al terminar seleccione la pestaña Browse, y agregue los siguientes paquetes Nuget en todas las plataformas.

- Newtonsoft.Json versión 12.0.3
- Refractored.MvvmHelpers versión 1.3.0
- Xamarin.Forms.Visual.Material de preferencia la misma versión que Xamarin.Forms.

En la siguiente imagen se muestra la instalación del paquete Newtonsoft.Json versión  12.0.3.

<div align="center">
<a href="/assets/images/post1_p10.png" data-fancybox>
<img src="/assets/images/post1_p10.png" /></a>
</div>

De un clic derecho sobre la el proyecto GifInMotion y busque la opción Add y seleccione New Folder, se creara un nuevo folder nómbrelo como Models, repita estos pasos para crear los Folders Services, ViewModels y Views.

<div align="center">
<a href="/assets/images/post1_p11.png" data-fancybox>
<img src="/assets/images/post1_p11.png" /></a>
</div>

Esta es la estructura que debe tener su proyecto en este momento.

<div align="center">
<a href="/assets/images/post1_p12.png" data-fancybox>
<img src="/assets/images/post1_p12.png" /></a>
</div>

Seleccione la carpeta Models de clic derecho Add y seleccione Class,  en la nueva ventana escriba Movies.cs para darle nombre a la clase y de Clic en el botón Add..

<div align="center">
<a href="/assets/images/post1_p13.png" data-fancybox>
<img src="/assets/images/post1_p13.png" /></a>
</div>

Ingrese el siguiente código para la clase Movies.cs

```csharp
using System;
using System.Collections.Generic;
using System.Globalization;
using Newtonsoft.Json;
using Newtonsoft.Json.Converters;


namespace GifInMotion.Models
{
    public partial class Movies
    {
        [JsonProperty("No")]
        public string No { get; set; }
        [JsonProperty("Title")]
        public string Title { get; set; }
        [JsonProperty("Link")]
        public string Link { get; set; }
        [JsonProperty("Episode")]
        public string Episode { get; set; }

    }

    public partial class Movies
    {
        public static Movies[] FromJson(string json) => JsonConvert.DeserializeObject<Movies[]>(json, Converter.Settings);
    }

    public static class Serialize
    {
        public static string ToJson(this Movies[] self) => JsonConvert.SerializeObject(self, Converter.Settings);
    }

    internal static class Converter
    {
        public static readonly JsonSerializerSettings Settings = new JsonSerializerSettings
        {
            MetadataPropertyHandling = MetadataPropertyHandling.Ignore,
            DateParseHandling = DateParseHandling.None,
            Converters = {new IsoDateTimeConverter { DateTimeStyles = DateTimeStyles.AssumeUniversal }
            },
        };
    }

}
```
Seleccione la carpeta Services de clic derecho Add, New Item, Seleccione la plantilla Interface, en Name escriba IDataService y de clic en el botón Add.

<div align="center">
<a href="/assets/images/post1_p14.png" data-fancybox>
<img src="/assets/images/post1_p14.png" /></a>
</div>

Copie el código de la interface.

```csharp
using System.Collections.Generic;
using System.Threading.Tasks;
using GifInMotion.Models;

namespace GifInMotion.Services
{
    public interface IDataService
    {
        Task<IEnumerable<Movies>> GetMoviesAsync();
    }
}
```
Seleccione la carpeta Services de clic derecho Add, Class, , en Name escriba WebDataService.cs y de clic en el botón Add.

<div align="center">
<a href="/assets/images/post1_p15.png" data-fancybox>
<img src="/assets/images/post1_p15.png" /></a>
</div>

Copie el código de la clase.

```csharp
using GifInMotion.Models;
using GifInMotion.Services;
using System.Collections.Generic;
using System.Net.Http;
using System.Threading.Tasks;
using Xamarin.Forms;

[assembly: Dependency(typeof(WebDataService))]
namespace GifInMotion.Services
{
    public class WebDataService : IDataService
    {
        HttpClient httpClient;

        HttpClient Client => httpClient ?? (httpClient = new HttpClient());

        public async Task<IEnumerable<Movies>> GetMoviesAsync()
        {
            var json = await Client.GetStringAsync("https://raw.githubusercontent.com/BryanOroxon/GifInMotion/master/GifInMotion/GifInMotion/Data/movie.json");
            var all = Movies.FromJson(json);
            return all;
        }
    }
}
```

Seleccione la carpeta ViewModels de clic derecho Add y seleccione Class,  en la nueva ventana escriba BaseViewModels.cs para darle nombre a la clase y de Clic en el botón Add.

<div align="center">
<a href="/assets/images/post1_p16.png" data-fancybox>
<img src="/assets/images/post1_p16.png" /></a>
</div>

Copie el código de la clase.

```csharp
using System.ComponentModel;
using System.Runtime.CompilerServices;
using Xamarin.Forms;
using GifInMotion.Services;

namespace GifInMotion.ViewModels
{
    public class BaseViewModel : INotifyPropertyChanged
    {
        public IDataService DataService { get; }
        public BaseViewModel()
        {
            DataService = DependencyService.Get<IDataService>();
        }

        bool isBusy;

        string title;
        public string Title
        {
            get => title;
            set
            {
                if (title == value)
                    return;
                title = value;
                OnPropertyChanged();
            }
        }
        public bool IsBusy
        {
            get => isBusy;
            set
            {
                if (isBusy == value)
                    return;

                isBusy = value;
                OnPropertyChanged();
                OnPropertyChanged(nameof(IsNotBusy));
            }
        }

        public bool IsNotBusy => !IsBusy;

        public event PropertyChangedEventHandler PropertyChanged;

        public void OnPropertyChanged([CallerMemberName]string name = null) => PropertyChanged?.Invoke(this, new PropertyChangedEventArgs(name));
    }
}
```

Seleccione la carpeta ViewModels de clic derecho Add y seleccione Class,  en la nueva ventana escriba MovieViewModel.cs para darle nombre a la clase y de Clic en el botón Add.

<div align="center">
<a href="/assets/images/post1_p17.png" data-fancybox>
<img src="/assets/images/post1_p17.png" /></a>
</div>

Copie el código de la clase.

```csharp
using System;
using System.Threading.Tasks;
using Xamarin.Forms;
using Xamarin.Essentials;

using System.Linq;
using GifInMotion.Models;
using System.Diagnostics;
using System.Collections.ObjectModel;
using MvvmHelpers;
using System.Windows.Input;

namespace GifInMotion.ViewModels
{
    public class MovieViewModel : BaseViewModel
    {
        public ObservableRangeCollection<Movies> Movies { get; }
        public Command<string> GetMoviesCommand { get; }

        public MovieViewModel()
        {
            Movies = new ObservableRangeCollection<Movies>();
            GetMoviesCommand = new Command<string>(async (test) => await GetMoviesAsync(test));
        }

        async Task GetMoviesAsync(string test)
        {
            if (IsBusy)
                return;
            try
            {
                IsBusy = true;
                var movies = await DataService.GetMoviesAsync();

                Movies.ReplaceRange(movies);

                Title = $"{test} ({Movies.Count})";
            }
            catch (Exception ex)
            {
                Debug.WriteLine($"Unable to get Movies: {ex.Message}");
                await Application.Current.MainPage.DisplayAlert("Error!", ex.Message, "OK");
            }
            finally
            {
                IsBusy = false;
            }
        }

    }
}
```

Ahora crearemos las vistas necesarias para consumir el servicio creado.

Seleccione la carpeta View de clic derecho Add, New Item, en la nueva ventana Seleccione la plantilla Content Page, en Name escriba CollectionMovies.xaml para darle nombre a la Content Page y de Clic en el botón Add.

<div align="center">
<a href="/assets/images/post1_p18.png" data-fancybox>
<img src="/assets/images/post1_p18.png" /></a>
</div>

En el Archivo CollectionMovies.xaml,  copie el código del xaml.

```xml
<?xml version="1.0" encoding="utf-8" ?>
<ContentPage
    x:Class="GifInMotion.Views.CollectionMovies"
    xmlns="http://xamarin.com/schemas/2014/forms&quot;
    xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml&quot;
    xmlns:d="http://xamarin.com/schemas/2014/forms/design&quot;
    xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006&quot;
    xmlns:viewmodel="clr-namespace:GifInMotion.ViewModels"
    Title="{Binding Title}"
    BackgroundImageSource="space.png"
    mc:Ignorable="d">
    <ContentPage.Content>
        <CollectionView x:Name="CollectionViewSource">
            <CollectionView.ItemTemplate>
                <DataTemplate>
                    <Grid>
                        <Grid.RowDefinitions>
                            <RowDefinition Height="25" />
                            <RowDefinition Height="250" />
                            <RowDefinition Height="25" />
                        </Grid.RowDefinitions>
                        <Grid.ColumnDefinitions>
                            <ColumnDefinition Width="*" />
                        </Grid.ColumnDefinitions>
                        <Label
                            Grid.Row="0"
                            FontAttributes="Bold"
                            FontSize="20"
                            Text="{Binding Title}"
                            TextColor="White" />
                        <Image
                            x:Name="GifXaml"
                            Grid.Row="1"
                            IsAnimationPlaying="True"
                            Source="{Binding Link}" />
                        <StackLayout Grid.Row="2" Orientation="Horizontal">
                            <Label
                                FontAttributes="Bold"
                                FontSize="20"
                                Text="Episode "
                                TextColor="White" />
                            <Label
                                FontAttributes="Bold"
                                FontSize="20"
                                Text="{Binding Episode}"
                                TextColor="White" />
                        </StackLayout>
                    </Grid>
                </DataTemplate>
            </CollectionView.ItemTemplate>
        </CollectionView>
    </ContentPage.Content>
</ContentPage>
```

Está vista, tendrá el titulo con un binding, usaremos el Viewmodel Movies, y con  un Grid se mostraran los datos del DataTemplate por medio de MVVM.

Todo archivo Xaml contiene un archivo con el mismo nombre .xaml.cs, de clic en el triángulo antes de CollectionMovies.xaml y seleccione el archivo CollectionMoviesl.xaml.cs.

<div align="center">
<a href="/assets/images/post1_p19.png" data-fancybox>
<img src="/assets/images/post1_p19.png" /></a>
</div>

Copie el código.

```csharp
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.Threading.Tasks;

using Xamarin.Forms;
using Xamarin.Forms.Xaml;
using GifInMotion.ViewModels;

namespace GifInMotion.Views
{
    [XamlCompilation(XamlCompilationOptions.Compile)]
    public partial class CollectionMovies : ContentPage
    {
        public MovieViewModel viewModel;
        public CollectionMovies()
        {
            InitializeComponent();          
            BindingContext = viewModel = new MovieViewModel();
            if (viewModel.Movies.Count == 0)
            {
                viewModel.GetMoviesCommand.Execute("Collection View");
                CollectionViewSource.ItemsSource = viewModel.Movies;
            }
        }
      
    }
}
```

En la ContentPage CollectionMovie, en el code behing enviaremos el ViewModel y el nombre de la página que es “Collection View” y se llenara el CollectionViewSource con los items de la fuente.

Seleccione la carpeta View de clic derecho Add, New Item, en la nueva ventana Seleccione la plantilla Content Page, en Name escriba MoviesCarousel.xaml, de Clic en el botón Add.

<div align="center">
<a href="/assets/images/post1_p20.png" data-fancybox>
<img src="/assets/images/post1_p20.png" /></a>
</div>

En el Archivo MoviesCarouse.xaml,  copie el código del xaml.

```xml
<?xml version="1.0" encoding="utf-8" ?>
<ContentPage
    x:Class="GifInMotion.Views.MoviesCarousel"
    xmlns="http://xamarin.com/schemas/2014/forms"
    xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
    xmlns:d="http://xamarin.com/schemas/2014/forms/design"
    xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
    xmlns:viewmodel="clr-namespace:GifInMotion.ViewModels"
    Title="{Binding Title}"
    BackgroundImageSource="space.png"
    mc:Ignorable="d">
    <ContentPage.Content>
        <CarouselView x:Name="movieViewSource">
            <CarouselView.ItemTemplate>
                <DataTemplate>
                    <StackLayout
                        HorizontalOptions="Center"
                        VerticalOptions="Center">
                        <Label
                            Text="{Binding Title}"
                            FontAttributes="Bold"
                            TextColor="White"
                            FontSize="20" />
                        <Image
                            x:Name="GifXaml"
                            Source="{Binding Link}"
                            IsAnimationPlaying="True"
                            WidthRequest="250"
                            HeightRequest="200" />
                        <StackLayout Orientation="Horizontal">
                            <Label
                                Text="Episode "
                                FontAttributes="Bold"
                                FontSize="20"
                                TextColor="White" />
                            <Label
                                Text="{Binding Episode}"
                                FontAttributes="Bold"
                                FontSize="20"
                                TextColor="White" />
                        </StackLayout>
                    </StackLayout>
                </DataTemplate>
            </CarouselView.ItemTemplate>
        </CarouselView>
    </ContentPage.Content>
</ContentPage>
```

De clic en el triángulo antes de MoviesCarousel.xaml y seleccione el archivo MoviesCarousel.xaml.cs.

<div align="center">
<a href="/assets/images/post1_p21.png" data-fancybox>
<img src="/assets/images/post1_p21.png" /></a>
</div>

Copie el código.

```csharp
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.Threading.Tasks;

using Xamarin.Forms;
using Xamarin.Forms.Xaml;
using GifInMotion.ViewModels;

namespace GifInMotion.Views
{
    [XamlCompilation(XamlCompilationOptions.Compile)]
    public partial class MoviesCarousel : ContentPage
    {
        public MovieViewModel viewModel;
        public MoviesCarousel()
        {
            InitializeComponent();
            BindingContext = viewModel = new MovieViewModel();
            if (viewModel.Movies.Count == 0)
            {
                viewModel.GetMoviesCommand.Execute("Carousel View");
                movieViewSource.ItemsSource = viewModel.Movies;
            }
        }
    }
}
```

Seleccione la carpeta View de clic derecho Add, New Item, en la nueva ventana Seleccione la plantilla  Content Page, en Name escriba HomePage.xaml, de Clic en el botón Add.

<div align="center">
<a href="/assets/images/post1_p22.png" data-fancybox>
<img src="/assets/images/post1_p22.png" /></a>
</div>

En el Archivo HomePage.xaml,  copie el código del xaml.

```xml
<?xml version="1.0" encoding="utf-8" ?>
<ContentPage
    x:Class="GifInMotion.Views.HomePage"
    xmlns="http://xamarin.com/schemas/2014/forms&quot;
    xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml&quot;
    xmlns:d="http://xamarin.com/schemas/2014/forms/design&quot;
    xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006&quot;
    Title="Gif In Motion"
    BackgroundImageSource="space.png"
    mc:Ignorable="d">
    <ContentPage.Content>

        <Frame
            BackgroundColor="Transparent"
            BorderColor="SpringGreen"
            CornerRadius="10">
            <Grid>
                <Grid.RowDefinitions>
                    <RowDefinition Height="20" />
                    <RowDefinition Height="*" />
                    <RowDefinition Height="35" />
                    <RowDefinition Height="35" />
                </Grid.RowDefinitions>
                <Grid.ColumnDefinitions>
                    <ColumnDefinition Width="*" />
                </Grid.ColumnDefinitions>
                <Label
                    CharacterSpacing="2"
                    FontAttributes="Bold"
                    HorizontalOptions="Center"
                    Text="Gif in Xamarin Forms!"
                    TextColor="White"
                    VerticalOptions="CenterAndExpand" />
                <Image
                    Grid.Row="1"
                    HeightRequest="600"
                    HorizontalOptions="CenterAndExpand"
                    IsAnimationPlaying="True"
                    Source="https://media.giphy.com/media/l1uguGf2RVIsTXNDO/giphy.gif&quot;
                    WidthRequest="800" />
                <Button
                    x:Name="BtnCollectionView"
                    Grid.Row="2"
                    Text="Collection View"
                    BackgroundColor="#00e5ff"
                    CornerRadius="18"
                    Clicked="BtnCollectionView_Clicked" />
                <Button
                    x:Name="BtnCarouselView"
                    Grid.Row="3"
                    Text="Carousel View"
                    BackgroundColor="#18ffff"
                    CornerRadius="18"
                    Clicked="BtnCarouselView_Clicked" />
            </Grid>
        </Frame>
    </ContentPage.Content>
</ContentPage>
```

De clic en el triángulo antes de HomePage.xaml y seleccione el archivo HomePage.xaml.cs y copie el código.

```csharp
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.Threading.Tasks;

using Xamarin.Forms;
using Xamarin.Forms.Xaml;

namespace GifInMotion.Views
{
    [XamlCompilation(XamlCompilationOptions.Compile)]
    public partial class HomePage : ContentPage
    {
        public HomePage()
        {
            InitializeComponent();
        }

        private async void BtnCarouselView_Clicked(object sender, EventArgs e)
        {
            await Navigation.PushAsync(new Views.MoviesCarousel());
        }

        private async  void BtnCollectionView_Clicked(object sender, EventArgs e)
        {
            await Navigation.PushAsync(new Views.CollectionMovies());
        }
    }
}
```

La Content Page HomePage mostrara un gif animado utilizando Image Source Directo y 2 botones los cuales nos mostrarán un CollectionView y el otro un CarouselView, que son las vistas que hemos creado previamente, las llamadas de navegación los agregamos con métodos asíncronos en el Code Behind.

Ya casi todo esta listo, ahora modificaremos el archivo App.xaml.cs para cambiar la página de inicio.

<div align="center">
<a href="/assets/images/post1_p23.png" data-fancybox>
<img src="/assets/images/post1_p23.png" /></a>
</div>

Modifica el Mainpage por:

MainPage = new NavigationPage( new Views.HomePage());

O copia el código.

```csharp
using System;
using Xamarin.Forms;
using Xamarin.Forms.Xaml;

namespace GifInMotion
{
    public partial class App : Application
    {
        public App()
        {
            InitializeComponent();

            MainPage = new NavigationPage( new Views.HomePage());
        }
        
        protected override void OnStart()
        {
        }

        protected override void OnSleep()
        {
        }

        protected override void OnResume()
        {
        }
    }
}
```

Descarga los Recursos para el Background de la App desde este link.

https://1drv.ms/u/s!AliWRcktwwLrha4OlCuyhpgbg0urCA?e=gGB2Vl

El archivo GifInMotion.zip contiene los recursos para las 3 plataformas.

<div align="center">
<a href="/assets/images/post1_p24.png" data-fancybox>
<img src="/assets/images/post1_p24.png" /></a>
</div>

Abra la carpeta GifInMotion.Android, y la carpeta Resources y copie todas las carpetas que contiene.

<div align="center">
<a href="/assets/images/post1_p25.png" data-fancybox>
<img src="/assets/images/post1_p25.png" /></a>
</div>

Selecciones el proyecto GifInMotion.Android, y de clic derecho en Resources y de clic en Paste (Pegar).

<div align="center">
<a href="/assets/images/post1_p26.png" data-fancybox>
<img src="/assets/images/post1_p26.png" /></a>
</div>

Se mostrará una ventana que indica que ya existe una carpeta con ese nombre, seleccione el “CheckBox Apply to all items” y de clic en Yes. (Si se muestra un mensaje que indique ya existe un archivo seleccioné “CheckBox Apply to all items” y de clic en Yes).

<div align="center">
<a href="/assets/images/post1_p27.png" data-fancybox>
<img src="/assets/images/post1_p27.png" /></a>
</div>

Abra la carpeta GifInMotion.iOS, y la carpeta Resources y copie todas los archivos que contiene.

<div align="center">
<a href="/assets/images/post1_p28.png" data-fancybox>
<img src="/assets/images/post1_p28.png" /></a>
</div>

En VS 2019  abra el proyecto GifInMotion.iOS, y la carpeta Resources y pegue los archivos copiados previamente.

<div align="center">
<a href="/assets/images/post1_p29.png" data-fancybox>
<img src="/assets/images/post1_p29.png" /></a>
</div>

En VS 2019 abra el proyecto GifInMotion.UWP. Para agregar la imagen se debe dar clic derecho sobre el proyecto GifInMotion.UWP en Add y seleccionar Existing Item.

<div align="center">
<a href="/assets/images/post1_p30.png" data-fancybox>
<img src="/assets/images/post1_p30.png" /></a>
</div>

Se abre una nueva ventana, busque donde haya extraído los archivos del zip, dentro de la carpeta  GifInMotion.UWP y seleccione el archivo space.png, para agregarlo al proyecto.

<div align="center">
<a href="/assets/images/post1_p31.png" data-fancybox>
<img src="/assets/images/post1_p31.png" /></a>
</div>

El código fuente puedes encontrarlo aquí:

<div align="center">
<a href="/assets/images/codigo_fuente.png" data-fancybox>
<img src="/assets/images/codigo_fuente.png" /></a>
</div>

El resultado del este proyecto debe verse así, con todos los pasos que hemos realizado este proyecto, funciona tanto en Android, iOS y Windows(UWP).

<p><iframe style="width:100%;" height="315" src="https://www.youtube.com/embed/7KlB5Z6y2y0?rel=0&amp;showinfo=0" frameborder="0" allowfullscreen></iframe></p>

## Conclusión

El soporte de Gif en Xamarin.Forms 4.4 funciona bastante bien, sólo recuerda que las imágenes se cargan cada vez que las visualices y no se puede hace caching de ellas, además que es necesario colocar la propiedad IsAnimationPlaying en True. Colocar un Gif animado en una interfaz de usuario, realza la aplicación, el CarouselView lo sentí más fluido que el CollectionView.

Este Post es parte del calendario de Adviento de la Comunidad Xamarin en Español, para más información sigue el link.

https://www.luisbeltran.mx/2019/11/06/primer-calendario-de-adviento-de-xamarin-en-espanol/

Muchas Gracias por darte el tiempo de leer este post.