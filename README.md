# django-nextjs

A barebones example of a Next.js SPA backed by a Django API.

Includes the following:

Backend:

- Django
- Django REST Framework
- JWT Authentication

Frontend:

- Next.js
- Tailwind

## Getting up and running

### Backend

Create a virtualenv:

```
$ python3 -m venv .venv
$ source .venv/bin/activate
```

Install requirements:

```
$ pip install -r requirements/base.txt
```

Set up DB (sqlite):

```
$ python manage.py makemigrations api
$ python manage.py migrate
```

### Frontend

See `../README.md`

## Running locally

```
$ source .venv/bin/activate
$ python manage.py runserver 4001
```

## Running tests

```
$ python manage.py test
```

## Deployment

Below is a quick overview on deploying the app to Heroku and Vercel.

### Notes on securing cookies

This project is configured so that the Next.js app and Django API are deployed separately. Whether they are deployed to different subdomains on the same second level domain (so something like Next.js -> www.example.com, Django -> api.example.com) or completely separate domains will affect how the refresh token cookie settings should be configured. This is because the former configuration results in [requests that are considered same-site](https://security.stackexchange.com/questions/223473/for-samesite-cookie-with-subdomains-what-are-considered-the-same-site) which allows us to set the SameSite attribute in the cookie to Lax. Otherwise, we need to set the SameSite to None.

### Backend

To deploy the backend on Heroku:

1. Create a new app on Heroku
2. Add Heroku Postgres
3. Connect the app to your github repo
4. Update the config variables (see below)
5. On the Deploy tab in Heroku, trigger a deploy manually from Github (or switch on automatic deploys if you want).

#### Backend config vars

- `SECRET_KEY`: see Django docs
- `DATABASE_URL`: set automatically when Postgres is added
- `CORS_ORIGIN_REGEX_WHITELIST`: A comma-separated list of origins ([ref](https://github.com/adamchainz/django-cors-headers#cors_origin_whitelist)). This should include the URL that the Next.js app gets deployed to (see below).
- `IGNORE_DOT_ENV_FILE=on`

### Frontend

To deploy the frontend on Vercel:

1. Click "Import Project"
2. Enter the URL of your github repo
3. Select the `www` subdirectory.
4. Add the `NEXT_PUBLIC_API_HOST` env var with the value set to the URL the Django API gets deployed to
5. Complete the build
