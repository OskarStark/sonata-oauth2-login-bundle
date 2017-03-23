# Exozet/Oauth2LoginBundle

### Installation

```console
    composer require silasjoisten/oauth2login-bundle
```

Register Bundle in **app/AppKernel.php**:
```php
 class AppKernel extends Kernel
 {
     public function registerBundles()
     {
        //...
        new HWI\Bundle\OAuthBundle\HWIOAuthBundle(),
        new Exozet\Oauth2LoginBundle\ExozetOauth2LoginBundle(),
//...
```

### Configuration

Include the Routing:
```yml
exozet_oauth:
    resource: "@ExozetOauth2LoginBundle/Resources/config/routing.xml"
```

Configure the HWIOauthBundle:

in security.yml:
```yml
security:
   providers:
      hwi:
         id: exozet.oauth2.user.provider

         #...

   firewalls:
      YOUR_FIREWALL:
         #pattern: /admin(.*) REMOVE THIS LINE IF YOU ARE USING SONATA ADMIN
         oauth:
            resource_owners:
               google:             "/login/check-google"
            login_path:        /admin/login                 # For Sonata Admin
            use_forward:       false
            default_target_path: /admin/dashboard           # For Sonata Admin
            failure_path:      /admin/login                 # For Sonata Admin
            oauth_user_provider:
               service:  exozet.oauth2.user.provider
```

in config.yml:
```yml
hwi_oauth:
    firewall_names: [YOUR_FIREWALL]
    resource_owners:
        google:
            type:                "google"
            client_id:           "YOUR_CLIENT_ID"
            client_secret:       "YOUR_CLIENT_SECRET"
            scope:               "email profile"
            options:
                csrf: true
                access_type:     offline
```

### Usage

To use The Exozet Login you just need to call the Twig function to render the button in your login template

```twig
    {{ render_exozet_login_button() }}
```

Optional: You can pass an *array* inside to to set custom *class* and *value*