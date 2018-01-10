# edge-scaffolding
This is an edge project scaffolding. Edge project is a deployable unit to iot edge hub. The project includes two parts. The first is **edge.deployment** file describe the deployment manifest of an edge device. And it is used to generate the final deployment.json file and sent to iot hub for edge device deployment. The second part is the **modules** folder, which contains the in-house developed module projects.

## "modules" Folder
Each folder under **modules** folder is an **Edge Module Project**.
### Edge Module Project (edgemodule.json)
1. Module is language specific. It is an implementation unit. The final output of the module project is a version managed image.
2. Each module will have a module descriptpion file "edgemodule.json". The main perpose of this file is to take care of the version management of the final image, and describe the special properties of the module.
3. The **Docker** folder under the project holds the DockerFiles per platforms. For example, the Docker file for linux-x64 platform should be at "./Docker/linux-x64/DockerFile".

## "edge.deployment" File
The deployment includes both third-party images and the self contained modules under modules folder. The file references modules thorugh docker image, and sets the runtime properties of each module. To avoid the continous version change of the self-contained modules (modules in "modules" folder) in this file, it uses a formated image variable to reference the self-contained module. (${modules.<modulename>.<platform>.image}) The variable will later be replaced by the exact image value and generate the final deploy manifest deployment.json file.

## Usage
1. Init the edge project with edge-scaffolding.
2. Add module to the modules folder. 
3. Make sure **edgemodule.json** of the new added module is configured properly especially the image. The tag of the image includes two parts: version and platform. For example:
```json
    "image": {
        "repository": "test.azurecr.io/filter-module",
        "tag": {
            "version": "0.0.1",
            "platforms": ["arm32v7", "linux-64", "windows-nano"]
        }
    }
```
This means, there will be three images generated for the edge module: test.azurecr.io/filter-module:0.0.1-arm32v7, test.azurecr.io/filter-module:0.0.1-linux-64, and test.azurecr.io/filter-module:windows-nano. Each one will match to the platform specific Docker file. (./Docker/<platform>/DockerFile)

4. Add module reference to edge.deployment. And fill in the image with image variable ({modules.<modulefoldername>.<platform>.image}), for example for module folder "./modules/datamodule" on linux-x64, the variable will be ${modules.datamodule.linux-x64.image}
