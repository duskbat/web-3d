<template>
  <div class="box">

  </div>
</template>

<script>
import * as THREE from 'three';
import {OrbitControls} from "three/examples/jsm/controls/OrbitControls";
import data from "@/views/3d/data.json";

export default {
  name: 'Web3D',

  mounted() {
    this.init(data);
  },

  methods: {
    init(data) {
      const scene = new THREE.Scene();
      const camera = new THREE.PerspectiveCamera(60, window.innerWidth / window.innerHeight, 1, 500);
      camera.position.z = 100;
      camera.position.x = 0;
      camera.position.y = 0;
      camera.lookAt(0, 0, 0);
      const renderer = new THREE.WebGLRenderer();
      renderer.setSize(window.innerWidth, window.innerHeight);
      document.body.appendChild(renderer.domElement);
      const controls = new OrbitControls(camera, renderer.domElement);
      controls.minDistance = 1;
      controls.maxDistance = 3000;
      controls.minPolarAngle = 0; //与水平夹角
      controls.maxPolarAngle = Math.PI / 2;


      // axis
      let axis = new THREE.AxesHelper(300);
      scene.add(axis);

      // glucose
      let glucose = data.glucose;
      let pointsParam = [];
      let childrenList = [];
      pointsParam.push(childrenList);

      let lastMinuteOffset;
      let zOffset = 5;
      for (let i in glucose) {
        let date = new Date(glucose[i].x);
        let hour = date.getHours();
        let minute = date.getMinutes();
        let minuteOffset = hour * 60 + minute;
        if (lastMinuteOffset > minuteOffset) {
          // create new pointList
          console.log(zOffset)
          zOffset += 2;
          childrenList = [];
          pointsParam.push(childrenList);
        }
        childrenList.push(new THREE.Vector3(minuteOffset / 5, 5 * glucose[i].y, zOffset));
        lastMinuteOffset = minuteOffset;
      }

      const colors = ["#FFFFFF", "#FFFF00", "#FF0000", "#16A085", "#2980B9", "#6F3889", "#BB796F", "#7E1A27", "#27AE60", "#FF6D00", "#167521", "#AE8CD1", "#FBD600", "#DD4E9F", "#73C5EA", "#829FA0", "#C3984E", "#44E12F", "#a34822", "#f41C91", "#2c2a73", "#225C31", "#acD633", "#872431", "#5cD213", "#5aE18F", "#5328a2", "#a9a19F", "#934912", "#EE9A49", "#EE8262", "#E066FF", "#339999", "#B4EEB4", "#B0E2FF", "#B03060", "#A4D3EE", "#9A32CD", "#98F5FF", "#949494", "#912CEE", "#8EE5EE", "#8B8B7A", "#6A5ACD", "#8B2500", "#8B1A1A", "#8B1C62", "#228B22", "#1F1F1F", "#00FF00", "#00CDCD", "#008B45", "#228B22", "#1F1F1F", "#00FF00", "#00CDCD", "#008B45", "#BDB76B", "#BCEE68", "#BCD2EE", "#BC8F8F", "#BBFFFF", "#BABABA", "#BA55D3", "#B9D3EE", "#B8B8B8", "#B8860B", "#B7B7B7", "#B5B5B5", "#B4EEB4", "#B4CDCD", "#B452CD", "#B3EE3A", "#B3B3B3", "#B2DFEE", "#B23AEE", "#B22222", "#B0E2FF", "#B0E0E6", "#B0C4DE", "#B0B0B0", "#B03060", "#AEEEEE", "#ADFF2F", "#ADD8E6", "#AB82FF", "#003333", "#A9A9A9", "#A8A8A8", "#A6A6A6", "#A52A2A", "#A4D3EE", "#A3A3A3", "#A2CD5A", "#A2B5CD", "#A1A1A1", "#A0522D", "#A020F0", "#9FB6CD", "#9F79EE", "#9E9E9E", "#9C9C9C", "#9BCD9B", "#9B30FF", "#9AFF9A", "#9ACD32", "#9AC0CD", "#9A32CD", "#999999", "#9932CC", "#98FB98", "#98F5FF", "#97FFFF", "#96CDCD", "#969696", "#949494", "#9400D3", "#9370DB", "#919191", "#912CEE", "#90EE90", "#8FBC8F", "#8F8F8F", "#8EE5EE", "#8E8E8E", "#8E8E38", "#8E388E", "#8DEEEE", "#8DB6CD", "#8C8C8C", "#8B8B83", "#8B8B7A", "#8B8B00", "#8B8989", "#8B8970", "#8B8878", "#8B8682", "#8B864E", "#8B8386", "#8B8378", "#8B814C", "#8B7E66", "#8B7D7B", "#8B7D6B", "#8B7B8B", "#990033", "#CC6699", "#FF6699", "#FF3366", "#993366", "#CC0066", "#CC0033", "#FF0066", "#FF0033", "#CC3399", "#FF3399", "#FF9999", "#FF99CC", "#FF0099", "#CC3366", "#FF66CC", "#FF33CC", "#FFCCFF", "#FF99FF", "#FF00CC", "#FF66FF", "#CC33CC", "#CC00FF", "#FF33FF", "#CC99FF", "#9900CC", "#FF00FF", "#CC66FF", "#990099", "#CC0099", "#CC33FF", "#CC99CC", "#990066", "#993399", "#CC66CC", "#CC00CC", "#663366", "#660099", "#666FFF", "#000CCC", "#9933CC", "#666699", "#660066", "#333366", "#0066CC", "#9900FF", "#333399", "#99CCFF", "#9933FF", "#330099", "#6699FF", "#9966CC", "#3300CC", "#003366", "#330033", "#3300FF", "#6699CC", "#663399", "#3333FF", "#006699", "#6633CC", "#3333CC", "#3399CC", "#6600CC", "#0066FF", "#0099CC", "#9966FF", "#0033FF", "#66CCFF", "#330066", "#3366FF", "#3399FF", "#6600FF", "#3366CC", "#003399", "#6633FF", "#000066", "#0099FF", "#CCCCFF", "#000033", "#33CCFF", "#9999FF", "#0000FF", "#00CCFF", "#9999CC", "#000099", "#6666CC", "#0033CC", "#FFFFCC", "#FFCC00", "#CC9909", "#663300", "#FF6600", "#663333", "#CC6666", "#FF6666", "#FFFF99", "#FFCC66", "#FF9900", "#FF9966", "#CC3300", "#996666", "#FFCCCC", "#660000", "#FF3300", "#FF6666", "#FFCC33", "#CC6600", "#FF6633", "#996633", "#CC9999", "#FF3333", "#990000", "#CC9966", "#FFFF33", "#CC9933", "#993300", "#FF9933", "#330000", "#993333", "#CC3333", "#CC0000", "#FFCC99", "#996600", "#CC6633", "#99FFFF", "#33CCCC", "#00CC99", "#99FF99", "#009966", "#33FF33", "#33FF00", "#99CC33", "#CCC333", "#66FFFF", "#66CCCC", "#66FFCC", "#66FF66", "#99FFCC", "#339933", "#33FF66", "#33CC33", "#99FF00", "#669900", "#666600", "#00FFFF", "#336666", "#00FF99", "#99CC99", "#00FF66", "#66FF33", "#66CC00", "#99CC00", "#999933", "#00CCCC", "#006666", "#339966", "#66FF99", "#CCFFCC", "#00CC00", "#CCFF66", "#CCCC66", "#009999", "#006633", "#33FF99", "#CCFF99", "#66CC33", "#33CC00", "#CCFF33", "#666633", "#669999", "#333333", "#336633", "#33CC66", "#99FF66", "#006600", "#339900", "#CCFF00", "#999966", "#666666", "#33FFCC", "#669966", "#00CC66", "#99FF33", "#003300", "#99CC66", "#999900", "#CCCC99", "#CCFFFF", "#33CC99", "#66CC66", "#66CC99", "#00FF33", "#009900", "#669900", "#669933", "#CCCC00", "#009933", "#00CC33", "#66FF00", "#336600", "#333000"];

      for (let i in pointsParam) {
        let pointList = pointsParam[i];
        console.log(pointList)
        const curve = new THREE.CatmullRomCurve3(
            pointList,
            false,
            "centripetal",
            0.5);
        let points = curve.getPoints(pointList.length);
        let lineGeometry = new THREE.BufferGeometry().setFromPoints(points);
        let lineMaterial = new THREE.LineBasicMaterial({color: colors[i]});
        let curveObject = new THREE.Line(lineGeometry, lineMaterial);
        scene.add(curveObject);
      }


      const geometry = new THREE.BoxGeometry(5, 5, 5);
      const material = new THREE.MeshNormalMaterial();
      const cube = new THREE.Mesh(geometry, material);
      cube.position.y = 5;
      scene.add(cube);


      function animate() {
        requestAnimationFrame(animate);

        cube.rotation.x += 0.01;
        cube.rotation.y += 0.05;

        renderer.render(scene, camera);
      }

      animate();
    }

  }
}
</script>

<style>
body {
  margin: 0;
}
</style>
