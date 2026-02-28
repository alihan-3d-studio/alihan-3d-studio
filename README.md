<!DOCTYPE html>
<html>
<head>
<meta charset="utf-8">
<title>Alihan 3D Studio v2</title>
<style>
body { margin:0; overflow:hidden; background:#111; color:white; font-family:Arial; }
#ui { position:absolute; top:10px; left:10px; background:#222; padding:15px; border-radius:10px; }
input { width:150px; }
button { margin-top:10px; padding:8px; cursor:pointer; }
</style>
</head>
<body>

<div id="ui">
<h3>Figür Ayarları</h3>

<label>Kafa Boyutu</label><br>
<input type="range" min="20" max="60" value="40" id="headSize"><br><br>

<label>Gövde Boyu</label><br>
<input type="range" min="30" max="80" value="50" id="bodySize"><br><br>

<label>Bacak Boyu</label><br>
<input type="range" min="20" max="60" value="35" id="legSize"><br><br>

<button onclick="exportSTL()">STL İndir</button>
</div>

<script src="https://cdnjs.cloudflare.com/ajax/libs/three.js/r128/three.min.js"></script>
<script src="https://cdn.jsdelivr.net/npm/three@0.128/examples/js/exporters/STLExporter.js"></script>
<script src="https://cdn.jsdelivr.net/npm/three@0.128/examples/js/controls/OrbitControls.js"></script>

<script>
let scene = new THREE.Scene();

let camera = new THREE.PerspectiveCamera(75, window.innerWidth/window.innerHeight, 0.1, 2000);
camera.position.set(0,100,250);

let renderer = new THREE.WebGLRenderer({antialias:true});
renderer.setSize(window.innerWidth, window.innerHeight);
document.body.appendChild(renderer.domElement);

let controls = new THREE.OrbitControls(camera, renderer.domElement);

let light = new THREE.DirectionalLight(0xffffff,1);
light.position.set(100,200,100);
scene.add(light);

let ambient = new THREE.AmbientLight(0x404040);
scene.add(ambient);

let material = new THREE.MeshStandardMaterial({color:0xaaaaaa});

let group = new THREE.Group();
scene.add(group);

function createFigure(headSize, bodySize, legSize){
    while(group.children.length > 0){ 
        group.remove(group.children[0]); 
    }

    let head = new THREE.Mesh(new THREE.SphereGeometry(headSize,32,32), material);
    head.position.y = bodySize + legSize + headSize;
    group.add(head);

    let body = new THREE.Mesh(new THREE.CylinderGeometry(20,20,bodySize,32), material);
    body.position.y = legSize + bodySize/2;
    group.add(body);

    let leg1 = new THREE.Mesh(new THREE.CylinderGeometry(12,12,legSize,32), material);
    leg1.position.set(-10, legSize/2, 0);
    group.add(leg1);

    let leg2 = new THREE.Mesh(new THREE.CylinderGeometry(12,12,legSize,32), material);
    leg2.position.set(10, legSize/2, 0);
    group.add(leg2);
}

createFigure(40,50,35);

function animate(){
    requestAnimationFrame(animate);
    renderer.render(scene,camera);
}
animate();

document.getElementById("headSize").oninput = update;
document.getElementById("bodySize").oninput = update;
document.getElementById("legSize").oninput = update;

function update(){
    createFigure(
        parseInt(headSize.value),
        parseInt(bodySize.value),
        parseInt(legSize.value)
    );
}

function exportSTL(){
    let exporter = new THREE.STLExporter();
    let result = exporter.parse(group); // sadece figür
    let blob = new Blob([result], {type: 'application/octet-stream'});
    let link = document.createElement('a');
    link.href = URL.createObjectURL(blob);
    link.download = 'alihan_figur.stl';
    link.click();
}
</script>

</body>
</html><img width="832" height="1248" alt="1771978287625" src="https://github.com/user-attachments/assets/15119ef5-b346-427e-960b-77a07c698cfa" />
<img width="1024" height="1024" alt="file_0000000085b4720c9e718de9da65b11b" src="https://github.com/user-attachments/assets/e4f9c399-73cb-4573-95a7-9f70b5a8f4ba" />
