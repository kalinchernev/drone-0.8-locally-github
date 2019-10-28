# Drone automation

[Drone 0.8 documentation](https://0-8-0.docs.drone.io)

Facilitates your local development workflows with Drone CI pipelines using Github client. There's one for [Gitlab](https://github.com/kalinchernev/drone-0.8-locally-gitlab).

## Requirements

- [Docker](https://www.docker.com/)
- [Docker Compose](https://docs.docker.com/compose/)
- [Github Application](https://docs.drone.io/installation/providers/github/)
- [ngrok](https://ngrok.com/)

## Setup

Here's a brief list of steps you need to take in order to start the automation infrastructure locally.

### Share local protocol and port

Simply run: (without the need of a running application on the port!)

```
$ ngrok http 80
```

This will start sharing your local `http` protocol via port 80. You will receive an address like `http://ada4e47d.ngrok.io`

This is necessary in order to enable integration between locally-run Drone server and external Oauth2 providers such as Gitlab or Github.

### Create Oauth2 application

[Follow the tutorial](https://docs.drone.io/installation/providers/github/) about creating an application, name is not important, but [`/authorize` callback is](https://0-8-0.docs.drone.io/install-for-github/).

When the application is created, save the values of `Application ID` and `Secret`.

### Start Drone

Clone `.env.example` file to `.env` and modify it. Or create a new file `.env` in the root folder. Set the appropriate values:

```
DRONE_HOST=http://ada4e47d.ngrok.io
DRONE_GITHUB_CLIENT=value of Application ID
DRONE_GITHUB_SECRET=value of Secret
DRONE_SECRET=value of your preference
```

Then, run the following:

```sh
$ docker-compose up
```

When the server is running, open `http://ada4e47d.ngrok.io` in your browser and authorize the application.

When authorized, [activate the project](https://0-8-0.docs.drone.io/getting-started/) in the web UI.

This activation is necessary for you to be able to configure the secrets and make use of the hooks attached for changes to trigger builds in the Drone automation system.

## Changing `DRONE_HOST`

An ngrok session is limited to a duration of 8 hours. When a session is restarted a new `DRONE_HOST` needs to be used.

Do the following:

- stop running Drone services
- update value in `.env`
- [update OAuth application](https://github.com/settings/developers)
- start Drone services
- toggle project's access to drone, i.e. `http://new-address.ngrok.io/account/repos`

## Troubleshooting

[check-config.sh](https://github.com/moby/moby/blob/master/contrib/check-config.sh) script might help.
