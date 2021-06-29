## Getting started

#### Install libraries (require node > 10.0.0 and npm > 6)
```
    npm install
```

#### Run project
```
    npm run start
```

## Interact with docker

#### Build Image
```
    docker image build -t imagename:version . 
```

#### Run container
```
    docker container run -p -d external_port:internal_port image_name
```