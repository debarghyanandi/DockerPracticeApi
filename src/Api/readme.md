# Docker Practice API

## Development Setup

### Dockerfile.dev

#### Build Command
```bash
docker build -f src/Api/Dockerfile.dev -t debarghya4/dockerapipractice .
```

#### Run Command
```bash
docker run -p 5000:8080 debarghya4/dockerapipractice
```

#### Access the API
Navigate to: `http://localhost:5000/swagger/index.html`

---

## Docker Compose

### Initial Build
```bash
docker compose up --build
### Build Output Example
```
[+] Building
 ✔ Image created
 ✔ Volume created
 ✔ Container running

Attaching to api-1
api-1  | dotnet watch ⌚ Polling file watcher is enabled
api-1  | dotnet watch 🔥 Hot reload enabled
api-1  | dotnet watch 🚀 Started
api-1  | info: Microsoft.Hosting.Lifetime[14]
api-1  |       Now listening on: http://0.0.0.0:8080
api-1  | Application started. Press Ctrl+C to shut down.
```

### Real-time Changes
Changes reflect in real-time with hot reload enabled.

---

## Production Dockerfile

### Build Command
```bash
docker build -f src/Api/Dockerfile -t debarghya4/dockerpracticeapi:0.1.0 .
```

### Build Output Example
```
[+] Building 0.9s (15/15) FINISHED
 => [internal] load build definition from Dockerfile               0.0s
 => [internal] load metadata for mcr.microsoft.com/dotnet/aspnet:8.0   0.1s
 => [internal] load metadata for mcr.microsoft.com/dotnet/sdk:8.0     0.4s
 => [buildstage 1/6] FROM mcr.microsoft.com/dotnet/sdk:8.0        0.0s
 => [final 1/3] FROM mcr.microsoft.com/dotnet/aspnet:8.0          0.0s
```
 => CACHED [final 2/3] WORKDIR /app                                                                                                                                     0.0s
 => CACHED [buildstage 2/6] WORKDIR /src/Api                                                                                                                            0.0s
 => CACHED [buildstage 3/6] COPY src/Api/*.csproj .                                                                                                                     0.0s
 => CACHED [buildstage 4/6] RUN dotnet  restore                                                                                                                         0.0s
 => CACHED [buildstage 5/6] COPY src/Api/. .                                                                                                                            0.0s
 => CACHED [buildstage 6/6] RUN dotnet publish -c Release -o /app/publish                                                                                               0.0s
 => CACHED [final 3/3] COPY --from=buildstage /app/publish .                                                                                                            0.0s
 => exporting to image                                                                                                                                                  0.1s
 => => exporting layers                                                                                                                                                 0.0s
 => => exporting manifest sha256:76840c0a5cddbd38c83a8dcfb04d14ab0062f83b2e859aaa8f17e8cf936377f9                                                                       0.0s
 => => exporting config sha256:22768bc408a010c8668c14d12256c9ccc1c965129d8a68c8024a7239e8bcde41                                                                         0.0s
 => => exporting attestation manifest sha256:cf820b4b9434ed185bbc0c34ce9c7dd08ba7fedf5ab98e34baf883506a96bb68                                                           0.0s
 => => exporting manifest list sha256:9e55c480064af46b09062c1dee228a690649b678fbc12bb159cdddf806bd460a                                                                  0.0s
 => => naming to docker.io/debarghya4/dockerpracticeapi:0.1.0                                                                                                           0.0s
 => => unpacking to docker.io/debarghya4/dockerpracticeapi:0.1.0                                                                                                        0.0s

View build details: docker-desktop://dashboard/build/desktop-linux/desktop-linux/nfnbay3saps906unzwtruh2a5

debar@FCBAYERN MINGW64 ~/Source/Repos/DockerH1(Module1-6)/DockerPracticeApi (master)
$ docker run -p 5000:8080 debarghya4/dockerpracticeapi:0.1.0
info: Microsoft.Hosting.Lifetime[14]
      Now listening on: http://[::]:8080
info: Microsoft.Hosting.Lifetime[0]
      Application started. Press Ctrl+C to shut down.
info: Microsoft.Hosting.Lifetime[0]
      Hosting environment: Production
info: Microsoft.Hosting.Lifetime[0]
      Content root path: /app


So in while testing prod had to remove this check (app.Environment.IsDevelopment()) because .net runs the env as production so swagger was not loading.
with out this removal we can directly call http://localhost:5000/WeatherForecast and get response [
  {
    "date": "2026-07-23",
    "temperatureC": 24,
    "temperatureF": 75,
    "summary": "Sweltering"
  },
  {
    "date": "2026-07-24",
    "temperatureC": 2,
    "temperatureF": 35,
    "summary": "Mild"
  },
  {
    "date": "2026-07-25",
    "temperatureC": -13,
    "temperatureF": 9,
    "summary": "Hot"
  },
  {
    "date": "2026-07-26",
    "temperatureC": 10,
    "temperatureF": 49,
    "summary": "Scorching"
  },
  {
    "date": "2026-07-27",
    "temperatureC": -8,
    "temperatureF": 18,
    "summary": "Hot"
  }
]

But the swagger was not loading.

and Logs reach 
CONTAINER ID   IMAGE                                COMMAND                  CREATED         STATUS         PORTS                                         NAMES
197f8b3ec1c5   debarghya4/dockerpracticeapi:0.1.0   "dotnet DockerPracti…"   3 minutes ago   Up 3 minutes   0.0.0.0:5000->8080/tcp, [::]:5000->8080/tcp   interesting_turing
PS C:\Users\debar\Source\Repos\DockerH1(Module1-6)\DockerPracticeApi> docker logs ^C
PS C:\Users\debar\Source\Repos\DockerH1(Module1-6)\DockerPracticeApi> docker logs 197f8b3ec1c5
info: Microsoft.Hosting.Lifetime[14]
      Now listening on: http://[::]:8080
info: Microsoft.Hosting.Lifetime[0]
      Application started. Press Ctrl+C to shut down.
info: Microsoft.Hosting.Lifetime[0]
      Hosting environment: Production
info: Microsoft.Hosting.Lifetime[0]
      Content root path: /app

      update log
      after fixing docker compose 
      

debar@FCBAYERN MINGW64 ~/Source/Repos/DockerH1(Module1-6)/DockerPracticeApi (fix/addressed-comments)
$ docker compose up --build
#1 [internal] load local bake definitions
#1 reading from stdin 617B done
#1 DONE 0.0s

#2 [internal] load build definition from Dockerfile.dev
#2 transferring dockerfile: 233B done
#2 DONE 0.0s

#3 [internal] load metadata for mcr.microsoft.com/dotnet/sdk:8.0
#3 DONE 0.5s

#4 [internal] load .dockerignore
#4 transferring context: 2B done
#4 DONE 0.0s

#5 [1/5] FROM mcr.microsoft.com/dotnet/sdk:8.0@sha256:89ce6291bde9acdf59594e79fb8277c6d84c46e4b1f5bf126a4f18766e4bd597
#5 resolve mcr.microsoft.com/dotnet/sdk:8.0@sha256:89ce6291bde9acdf59594e79fb8277c6d84c46e4b1f5bf126a4f18766e4bd597 0.0s done
#5 DONE 0.0s

#6 [internal] load build context
#6 transferring context: 21.65kB 0.0s done
#6 DONE 0.0s

#7 [2/5] WORKDIR /src/Api
#7 CACHED

#8 [3/5] COPY src/Api/*.csproj .
#8 CACHED

#9 [4/5] RUN dotnet  restore
#9 CACHED

#10 [5/5] COPY src/Api/. .
#10 DONE 0.1s

#11 exporting to image
#11 exporting layers
#11 exporting layers 0.8s done
#11 exporting manifest sha256:e72ca8dae31abed2c08566fada72c1cf443352e96402713b95f8e593fc633c87 0.0s done
#11 exporting config sha256:d991a52addcfc24701a28c020695e4f0fdeb166dd8c2418194105b1657628da5 0.0s done
#11 exporting attestation manifest sha256:055f67fb7a4568bd70e3e2a110b8a13bb180f2950c886312d76762c5bf487886 0.0s done
#11 exporting manifest list sha256:38c4da29462ca76c46aad647c79a88ff2ba900762a01660a1a38116e526f7125 0.0s done
#11 naming to docker.io/library/dockerpracticeapi-api:latest done
#11 unpacking to docker.io/library/dockerpracticeapi-api:latest
#11 unpacking to docker.io/library/dockerpracticeapi-api:latest 0.3s done
#11 DONE 1.2s

#12 resolving provenance for metadata file
#12 DONE 0.0s
[+] up 2/2
 ✔ Image dockerpracticeapi-api       Built                                                                                                                                                                                                                                 2.3s
 ✔ Container dockerpracticeapi-api-1 Recreated                                                                                                                                                                                                                             0.2s
Attaching to api-1
api-1  | dotnet watch ⌚ Polling file watcher is enabled
api-1  | dotnet watch 🔥 Hot reload enabled. For a list of supported edits, see https://aka.ms/dotnet/hot-reload.
api-1  |   💡 Press "Ctrl + R" to restart.
api-1  | dotnet watch 🔧 Building...
api-1  |   Determining projects to restore...
api-1  |   All projects are up-to-date for restore.
api-1  |   DockerPracticeApi -> /src/Api/bin/Debug/net8.0/DockerPracticeApi.dll
api-1  | dotnet watch 🚀 Started
api-1  | warn: Microsoft.AspNetCore.Hosting.Diagnostics[15]
api-1  |       Overriding HTTP_PORTS '8080' and HTTPS_PORTS ''. Binding to values defined by URLS instead 'http://0.0.0.0:8080'.
api-1  | info: Microsoft.Hosting.Lifetime[14]
api-1  |       Now listening on: http://0.0.0.0:8080
api-1  | dotnet watch 🌐 Unable to launch the browser. Navigate to http://0.0.0.0:8080
api-1  | info: Microsoft.Hosting.Lifetime[0]
api-1  |       Application started. Press Ctrl+C to shut down.
api-1  | info: Microsoft.Hosting.Lifetime[0]
api-1  |       Hosting environment: Development
api-1  | info: Microsoft.Hosting.Lifetime[0]
api-1  |       Content root path: /src/Api


update log Dockerfile
now running without the gaurd 
debar@FCBAYERN MINGW64 ~/Source/Repos/DockerH1(Module1-6)/DockerPracticeApi (fix/addressed-comments)
$ docker run -e ASPNETCORE_ENVIRONMENT=Development -p 5000:8080 debarghya4/dockerpracticeapi:0.1.0
info: Microsoft.Hosting.Lifetime[14]
      Now listening on: http://[::]:8080
info: Microsoft.Hosting.Lifetime[0]
      Application started. Press Ctrl+C to shut down.
info: Microsoft.Hosting.Lifetime[0]
      Hosting environment: Development
info: Microsoft.Hosting.Lifetime[0]
      Content root path: /app

