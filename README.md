# Taiga Docker

&gt; **READ THIS FIRST!**: We recently announced Taiga plans for the future and they greatly affect how we manage this repository and the current Taiga 6 release. Check it [here](https://blog.taiga.io/announcing_taiganext.html).

## Getting Started

This section intends to explain how to get Taiga up and running in a simple two steps, using **docker** and **docker-compose**.

If you don't have docker installed, please follow installation instructions from docker.com: https://docs.docker.com/engine/install/

Additionally, it's necessary to have familiarity with Docker, docker-compose and Docker repositories.

**Note** branch `stable` should be used to deploy Taiga in production and `main` branch for development purposes.

### Start the application

```sh
$ ./launch-all.sh
```

After some instants, when the application is started you can proceed to create the superuser with the following script:

```sh
$ ./taiga-manage.sh createsuperuser
```

The `taiga-manage.sh` script lets launch manage.py commands on the
back instance:

```sh
$ ./taiga-manage.sh [COMMAND]
```

If you're testing it in your own machine, you can access the application in **http://localhost:9000**. If you're deploying in a server, you'll need to configure hosts and nginx as described later.

![Taiga screenshot](imgs/taiga.jpg)

As **EXTRA**: the default `launch-all.sh` script comes with [penpot](https://penpot.app), the open-source solution for design and prototyping. The default access for the penpot application is **http://localhost:9001**

It's developed by the same team behind Taiga. If you want to give it a try, you can go to [penpot's github](https://github.com/penpot/penpot) or the [help center](https://help.penpot.app/technical-guide/configuration/) to review its own configuration variables.

![Penpot screenshot](imgs/penpot.jpg)

And finally if you just want to launch Taiga standalone, you can use the `launch-taiga.sh` script instead of the `launch-all.sh`.

## Documentation

Currently, we have authored three main documentation hubs:

- **[API](https://docs.taiga.io/api.html)**: Our API documentation and reference for developing from Taiga API.
- **[Documentation](https://docs.taiga.io/)**: If you need to install Taiga on your own server, this is the place to find some guides.
- **[Taiga Community](https://community.taiga.io/)**: This page is intended to be the support reference page for the users.

## Bug reports

If you **find a bug** in Taiga you can always report it:

- in [Taiga issues](https://tree.taiga.io/project/taiga/issues). **This is the preferred way**
- in [Github issues](https://github.com/kaleidos-ventures/taiga-docker/issues)
- send us a mail to support@taiga.io if is a bug related to [tree.taiga.io](https://tree.taiga.io)
- send us a mail to security@taiga.io if is a **security bug**

One of our fellow Taiga developers will search, find and hunt it as soon as possible.

Please, before reporting a bug, write down how can we reproduce it, your operating system, your browser and version, and if it's possible, a screenshot. Sometimes it takes less time to fix a bug if the developer knows how to find it.

## Community

If you **need help to setup Taiga**, want to **talk about some cool enhancemnt** or you have **some questions**, please go to [Taiga community](https://community.taiga.io/).

If you want to be up to date about announcements of releases, important changes and so on, you can subscribe to our newsletter (you will find it by scrolling down at [https://taiga.io](https://www.taiga.io/)) and follow [@taigaio](https://twitter.com/taigaio) on Twitter.

## Contribute to Taiga

There are many different ways to contribute to Taiga's platform, from patches, to documentation and UI enhancements, just find the one that best fits with your skills. Check out our detailed [contribution guide](https://community.taiga.io/t/how-can-i-contribute/159#code-patches-enhacements-3).

## Code of Conduct

Help us keep the Taiga Community open and inclusive. Please read and follow our [Code of Conduct](https://github.com/kaleidos-ventures/code-of-conduct/blob/main/CODE_OF_CONDUCT.md).

## License

Every code patch accepted in Taiga codebase is licensed under [MPL 2.0](LICENSE). You must be careful to not include any code that can not be licensed under this license.

Please read carefully [our license](LICENSE) and ask us if you have any questions as well as the [Contribution policy](https://github.com/kaleidos-ventures/taiga-docker/blob/main/CONTRIBUTING.md).

## Configuration with Environment Variables

We've exposed the ** Basic configuration ** and the most commonly modified settings in Taiga to an `.env` file. We strongly recommend you to change it, or at least review its content, to avoid using the default values.

Both `docker-compose.yml` and `docker-compose-inits.yml` will read from this file to populate their environment variables, so, initially you don't need to change them. Edit these files just in case you require to enable **Additional customization**, or an **Advanced configuration**.

Refer to these sections for further information.

## Basic Configuration

You will find basic **configuration variables** in the `.env` file. As stated before, we encourage you to edit these values, especially those affecting the security.

### Database settings

These vars are used to create the database for Taiga and connect to it.

```bash
POSTGRES_DB=taiga  # PostgreSQL database name
POSTGRES_USER=taiga  # user to connect to PostgreSQL
POSTGRES_PASSWORD=taiga  # database user's password
```

### URLs settings

These vars set where your Taiga instance should be served, and the security protocols to use in the communication layer.

```bash
TAIGA_SCHEME=http  # serve Taiga using "http" or "https" (secured) connection
TAIGA_DOMAIN=localhost:9000  # Taiga's base URL
SUBPATH=""  # it'll be appended to the TAIGA_DOMAIN (use either "" or a "/subpath")
WEBSOCKETS_SCHEME=ws  # events connection protocol (use either "ws" or "wss")
```

The default configuration assumes Taiga is being served in a **subdomain**. For example:

```bash
TAIGA_SCHEME=https
TAIGA_DOMAIN=taiga.mycompany.com
SUBPATH=""
WEBSOCKETS_SCHEME=wss
```

If Taiga is being served in a **subpath**, instead of a subdomain, the configuration should be something like this:

```bash
TAIGA_SCHEME=https
TAIGA_DOMAIN=mycompany.com
SUBPATH="/taiga"
WEBSOCKETS_SCHEME=wss
```

### Secret Key settings

This variable allows you to set the secret key in Taiga, used in the cryptographic signing.

```bash
SECRET_KEY="taiga-secret-key"  # Please, change it to an unpredictable value!
```

### Email Settings

BBy default, emails will be printed in the standard output (`EMAIL_BACKEND=**console**`). If you have your own SMTP service, change it to `EMAIL_BACKEND=**smtp**` and configure the rest of these variables with the values supplied by your SMTP provider:

```bash
EMAIL_BACKEND=console  # use an SMTP server or display the emails in the console (either "smtp" or "console")
EMAIL_HOST=smtp.host.example.com  # SMTP server address
EMAIL_PORT=587   # default SMTP port
EMAIL_HOST_USER=user  # user to connect the SMTP server
EMAIL_HOST_PASSWORD=password  # SMTP user's password
EMAIL_DEFAULT_FROM=changeme@example.com  # email address for the automated emails

# EMAIL_USE_TLS/EMAIL_USE_SSL are mutually exclusive (only set one of those to True)
EMAIL_USE_TLS=True  # use TLS (secure) connection with the SMTP server
EMAIL_USE_SSL=False  # use implicit TLS (secure) connection with the SMTP server
```

### Queue manager settings

These variables are used to leave messages in the rabbitmq services.

```bash
RABBITMQ_USER=taiga  # user to connect to RabbitMQ
RABBITMQ_PASS=taiga  # RabbitMQ user's password
RABBITMQ_VHOST=taiga  # RabbitMQ container name
RABBITMQ_ERLANG_COOKIE=secret-erlang-cookie  # unique value shared by any connected instance of RabbitMQ
```

### Attachments settings

You can configure how long the attachments will be accessible by changing the token expiration timer. After that amount of seconds the token will expire, but you can always get a new attachment url with an active token.

```bash
ATTACHMENTS_MAX_AGE=360  # token expiration date (in seconds)
```


### Telemetry Settings

Telemetry anonymous data is collected in order to learn about the use of Taiga and improve the platform based on real scenarios.

```bash
ENABLE_TELEMETRY=True
```

You can opt out by setting this variable to False. By default, it's True.

## Customisation

All these features are disabled by default. You should add the corresponding environment variables with a proper value to enable them.

### Registration Settings

**Service: taiga-back**

```
PUBLIC_REGISTER_ENABLED: "True"
```

**Service: taiga-front**

```
PUBLIC_REGISTER_ENABLED: "true"
```

If you want to allow a public register, configure this variable to "True". By default is "False". Should be the same as this var in `taiga-front` and `taiga-back`.

**Important**: Taiga (in its default configuration) disables both Gitlab or Github oauth buttons whenever the public registration option hasn't been activated. To be able to use Github/ Gitlab login/registration, make sure you have public registration activated on your Taiga instance.

### Slack Settings

**Service: taiga-back**

```
ENABLE_SLACK: "True"
```

**Service: taiga-front**

```
ENABLE_SLACK: "true"
```

Enable Slack integration in your Taiga instance. By default is "False". Should have the same value as this variable in `taiga-front` and `taiga-back`.

### Github settings

Used for login with Github.
Get these in your profile https://github.com/settings/apps or in your organization profile https://github.com/organizations/{ORGANIZATION-SLUG}/settings/applications

**Note** `ENABLE_GITHUB_AUTH` and `GITHUB_CLIENT_ID` should have the same value in `taiga-back` and `taiga-front` services.

```
ENABLE_GITHUB_AUTH: "True"
GITHUB_API_CLIENT_ID: "github-api-client-id"
GITHUB_API_CLIENT_SECRET: "github-api-client-secret"
```

**Service: taiga-front**

```
ENABLE_GITHUB_AUTH: "true"
GITHUB_API_CLIENT_ID: "github-api-client-id"
```

### Gitlab settings

Used for login with GitLab.
Get these in your profile https://{YOUR-GITLAB}/profile/applications or in your organization profile https://{YOUR-GITLAB}/admin/applications

**Note** `ENABLE_GITLAB_AUTH`, `GITLAB_CLIENT_ID` and `GITLAB_URL` should have the same value in `taiga-back` and `taiga-front` services.

**Service: taiga-back**

```
ENABLE_GITLAB_AUTH: "True"
GITLAB_API_CLIENT_ID: "gitlab-api-client-id"
GITLAB_API_CLIENT_SECRET: "gitlab-api-client-secret"
GITLAB_URL: "gitlab-url"
```

**Service: taiga-front**

```
ENABLE_GITLAB_AUTH: "true"
GITLAB_CLIENT_ID: "gitlab-client-id"
GITLAB_URL: "gitlab-url"
```

### Importers

#### Github

**Service: taiga-back**

```
ENABLE_GITHUB_IMPORTER: "True"
GITHUB_IMPORTER_CLIENT_ID: "client-id-from-github"
GITHUB_IMPORTER_CLIENT_SECRET: "client-secret-from-github"
```

**Service: taiga-front**

```
ENABLE_GITHUB_IMPORTER: "true"
```

#### Jira

**Service: taiga-back**

```
ENABLE_JIRA_IMPORTER: "True"
JIRA_IMPORTER_CONSUMER_KEY: "consumer-key-from-jira"
JIRA_IMPORTER_CERT: "cert-from-jira"
JIRA_IMPORTER_PUB_CERT: "pub-cert-from-jira"
```

**Service: taiga-front**

```
ENABLE_JIRA_IMPORTER: "true"
```

#### Trello

**Service: taiga-back**

```
ENABLE_TRELLO_IMPORTER: "True"
TRELLO_IMPORTER_API_KEY: "api-key-from-trello"
TRELLO_IMPORTER_SECRET_KEY: "secret-key-from-trello"
```

**Service: taiga-front**

```
ENABLE_TRELLO_IMPORTER: "true"
```

## Storage

We have 3 named volumes configured: `taiga-static-data` for statics, `taiga-media-data` for medias and `taiga-db-data` for the database.

## Advanced customization (via configuration files)

For a advanced customization, you can use configuration files, mapped to specific directories inside the containers.

### taiga-back

Map a Python configuration file to `/taiga-back/settings/config.py`. You can use [this file](https://raw.githubusercontent.com/kaleidos-ventures/taiga-back/main/docker/config.py) as an example.

**Important**: if you use your own configuration file, don't forget to add contribs to `INSTALLED_APPS` (check the example `config.py`).

### taiga-front

Map a `conf.json`configuration file to `/usr/share/nginx/html/conf.json`. You can use [this file](https://raw.githubusercontent.com/kaleidos-ventures/taiga-front/main/docker/conf.json.template) as an example.
