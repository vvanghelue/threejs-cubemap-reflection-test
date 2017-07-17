# threejs-cubemap-reflection-test

demo https://vvanghelue.github.io/threejs-cubemap-reflection-test/

```javascript
renderer.shadowMap.enabled = true;
renderer.shadowMap.type = THREE.PCFSoftShadowMap;

var light = this.getObjectByName('DirectionalLight 1');
light.shadow.camera.left = -50;
light.shadow.camera.right = 50;
light.shadow.camera.top = 50;
light.shadow.camera.bottom = -50;
light.shadow.camera.updateProjectionMatrix(); // important!

//light.shadow.radius = 10;  

light.shadow.bias = 0.0001;  
light.shadow.mapSize.width = 256;  
light.shadow.mapSize.height = 256; 

// default//var helper = new THREE.CameraHelper( light.shadow.camera );
//scene.add( helper );

var torus = this.getObjectByName('TorusKnot 1');
var wall1 = this.getObjectByName('Box 1');
var cubeCamera1 = new THREE.CubeCamera( 1, 1000, 128 );
cubeCamera1.renderTarget.texture.minFilter = THREE.LinearMipMapLinearFilter;
this.add( cubeCamera1 );

torus.material.envMap = cubeCamera1.renderTarget.texture;
var cubeCamera2 = new THREE.CubeCamera( 1, 1000, 128 );
cubeCamera2.renderTarget.texture.minFilter = THREE.LinearMipMapLinearFilter;
this.add( cubeCamera2 );
wall1.material.envMap = cubeCamera2.renderTarget.texture;

var basePositionX = torus.position.x;
var basePositionZ = torus.position.z;
var basePositionCameraX = camera.position.x;
var basePositionCameraZ = camera.position.z;
var i = 0;
update = function( event ) {
	torus.position.x = basePositionX + 1 * Math.sin(new Date().getTime() / 1000);
	torus.position.z = basePositionZ + 15 * Math.cos(new Date().getTime() / 1000);
	camera.position.x = basePositionCameraX + 2 * Math.cos(new Date().getTime() / 1000);
	camera.position.z = basePositionCameraZ + 3 * Math.sin(new Date().getTime() / 1000);
	//if (i%2 == 0) {
		torus.visible = false;
		cubeCamera1.position.copy(torus.position);
		cubeCamera1.updateCubeMap( renderer, this );
		torus.visible = true;
	//}
	
	//wall1.visible = false;
	//cubeCamera2.position.copy(wall1.position);
	//cubeCamera2.updateCubeMap( renderer, this );
	//wall1.visible = true;

	i++;
}.bind(this);
```
