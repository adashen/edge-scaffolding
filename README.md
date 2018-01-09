# edge-scaffolding
The this is an edge project scaffolding. Edge project is a deployable unit to iot edge hub. Thus the project includes two main parts. The first is edge.deployment file describe the deployment of an edge device. And can be used to generate the final deployment.json file and sent to iot hub for deployment. The second part is the modules folder, which contains the user module projects.

## modules
Each folder under modules defines an edge module.
### module (edgemodule.json)
1. Module is language specific. It is an implementation unit, and the final output of the module is a version managed image.
2. Each module will have a module descriptpion file "edgemodule.json". The main perpose of this file is to take care of the version management of the final image, and describe the special properties of the module.

## edge.deployment
The deployment includes both third-party images and the self contained modules under modules folder. The file references modules thorugh docker image, and describe the runtime properties of each module. To avoid the continous version change of the self-contained modules in this file, it uses a formated image variable to reference the self contained module. (${modules.<modulename>.<os>.image}) The variable will later be replaced by the exact image value and generate the final deployment.json file.

## Usage
1. Init the edge project with edge-scaffolding.
2. Add module to the modules folder. And make sure edgemodule.json is well defined.
3. Add module reference to edge.deployment. And fill in the image with image variable ({modules.<modulename>.<os>.image}), for example for module "datamodule" on linux-x64, the variable will be ${modules.datamodule.linux-x64.image}
