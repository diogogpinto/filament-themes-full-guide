# Filament Theme - The Full Guide
A Full Guide on how to create a custom Filament Theme for your Filament v3.0 Panels.

## Resources
- [Filament Docs](https://filamentphp.com/docs/)
- [Filament Official Documentation on Themes](https://filamentphp.com/docs/3.x/panels/themes#creating-a-custom-theme)

## Before You Start

I am assuming you already have a Filament Panel setup. If you don't have one, please setup a 
panel using the [official documentation guide](https://filamentphp.com/docs/3.x/panels/getting-started).

## Creating a theme

In this example, we will be creating a custom theme for a Panel named `cms`.

### Creating the theme using the terminal

In your project's root directory, run:

```bash
php artisan make:filament-theme
```

It will scan your app for all the panels you have created and let you select which panel this themes applies to. Select the panel it belongs to. In this particular case, we will select the panel `cms`.

![Create Theme Terminal Command](./screenshots/create-theme.png)

After you've selected the panel, the terminal will give you all further instructions, but we'll go for each one step by step.

![After Creating the Theme](./screenshots/after-create-theme.png)

#### Editing vite.config.js

Go to the `vite.config.js` file in your project's root directory and set the input array like below:

```javascript
import { defineConfig } from 'vite'
import laravel, { refreshPaths } from 'laravel-vite-plugin'

export default defineConfig({
    plugins: [
        laravel.default({
            input: [
                'resources/css/app.css',
                'resources/js/app.js',
                'resources/css/filament/cms/theme.css'
            ],
            refresh: [
                ...refreshPaths,
                'app/Filament/**',
                'app/Forms/Components/**',
                'app/Livewire/**',
                'app/Infolists/Components/**',
                'app/Providers/Filament/**',
                'app/Tables/Columns/**',
            ],
        }),
    ],
})
```

See how we've added `resources/css/filament/cms/theme.css` to that array? This will tell vite where our custom theme is located when running the build process.

>[!note]
> If you have other themes, you can add them in the same array

#### Registering the viteTheme in our panel

Now we shall edit the `CmsPanelProvider.php` fila and add the viteTheme method to our `$panel`, like the example below:

```php
        return $panel
            ->id('cms')
            ->path('cms')
            ->viteTheme('resources/css/filament/cms/theme.css')
```

Our panel now knows we are using a custom theme registered under that path.

#### Running the build process

At last, we shall run the build process with the terminal in our project's root directory:

```bash
npm run build
```