CartoView is a GIS Web Mapping Application Market.

Cartoview extends the popular [GeoNode](http://geonode.org/) SDI to provide the ability to create, share, and visualize GIS Web Mapping Applications very easily and very quickly from the browser without programming.

## Docker Installation:
  - install [docker](https://docs.docker.com/v17.12/install/#server) and [docker-compose](https://docs.docker.com/compose/install/#prerequisites)
  - clone cartoview and navigate to cartoview folder
  - on linux based OS use this command `make run` to setup and start cartoview in docker for the first time 
  - on windows run the following commands to setup and start cartoview in docker for the first time:
      ```sh
      $ docker-compose up
      $ docker-compose exec cartoview python manage.py makemigrations
      $ docker-compose exec cartoview python manage.py migrate
      $ docker-compose exec cartoview python manage.py loaddata sample_admin.json
      $ docker-compose exec cartoview python manage.py loaddata scripts/json/default_oauth_apps.json
      $ docker-compose exec cartoview python manage.py loaddata app_stores.json
      $ docker-compose exec cartoview python manage.py loaddata initial_data.json
      ```
  - open your browser and type the following address `10.5.0.4`
  - default user credentials `admin/admin` for cartoview and `admin/geoserver` for geoserver
  - you need to configure oauth in geonode and geoserver to do this please use this [link](http://docs.geonode.org/en/master/tutorials/admin/geoserver_geonode_security/index.html)
  - you can stop containers with `make down` or `docker-compose down`
  - you can get logs for each service in `docker-compose.yml` unsing the following command:
      - `docker-compose logs --follow --tail=100 <service_name>`
  - start the containers with `docker-compose up -d` or `make up`
  - stop the containers with `docker-compose down` or `make down`

## How To Add Cartoview To Existing Geonode:
  - install cartoview with pip:
      - `pip install cartoview --no-cache-dir`
      - open geonode `settings.py` and add the following lines at the end of the file:
          ```python
          
          from cartoview import settings as cartoview_settings
          
          
          INSTALLED_APPS = cartoview_settings.INSTALLED_APPS
          ROOT_URLCONF = cartoview_settings.ROOT_URLCONF


          APPS_DIR = os.path.abspath(os.path.join(PROJECT_ROOT, "apps"))
          PENDING_APPS = os.path.join(PROJECT_ROOT, "pendingOperation.yml")


          APPS_MENU = False

          
          TEMPLATES[0]["DIRS"] = CARTOVIEW_TEMPLATE_DIRS
          TEMPLATES[0]["OPTIONS"]['context_processors'] += cartoview_settings.CARTOVIEW_CONTEXT_PROCESSORS


          STATICFILES_DIRS += cartoview_settings.CARTOVIEW_STATIC_DIRS


          from cartoview.app_manager.settings import load_apps
          INSTALLED_APPS += load_apps()
          ```
      - restart your server

## Docs:
  - [How to use and install](http://cartologic.github.io)
