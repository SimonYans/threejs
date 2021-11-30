<template>
  <div class="three">
    <div id="threeCanvas"></div>
  </div>
</template>

<script>
import * as THREE from 'three'
import { OrbitControls } from 'three/examples/jsm/controls/OrbitControls'
export default {
  name: 'App',
  mounted() {
    // this.initThree()
    this.learnThree()
  },
  methods: {
    learnThree () {
      const scene = new THREE.Scene();

      const geometry = new THREE.BufferGeometry(); //声明一个缓冲几何体对象

      //类型数组创建顶点位置position数据
      const vertices = new Float32Array([
        0, 0, 0, //顶点1坐标
        50, 0, 0, //顶点2坐标
        0, 100, 0, //顶点3坐标

        0, 0, 10, //顶点4坐标
        0, 0, 100, //顶点5坐标
        50, 0, 10, //顶点6坐标
      ]);
      // 创建属性缓冲区对象
      const attribue = new THREE.BufferAttribute(vertices, 3); //3个为一组，作为一个顶点的xyz坐标
      // 设置几何体attributes属性的位置position属性
      geometry.attributes.position = attribue;
      //类型数组创建顶点颜色color数据
      const colors = new Float32Array([
        1, 0, 0, //顶点1颜色
        0, 1, 0, //顶点2颜色
        0, 0, 1, //顶点3颜色

        1, 1, 0, //顶点4颜色
        0, 1, 1, //顶点5颜色
        1, 0, 1, //顶点6颜色
      ]);
      // 设置几何体attributes属性的颜色color属性
      geometry.attributes.color = new THREE.BufferAttribute(colors, 3); //3个为一组,表示一个顶点的颜色数据RGB
      //材质对象
      const material = new THREE.PointsMaterial({
        // 使用顶点颜色数据渲染模型，不需要再定义color属性
        // color: 0xff0000,
        vertexColors: THREE.VertexColors, //以顶点颜色为准
        size: 10.0 //点对象像素尺寸
      });
      // 点渲染模式  点模型对象Points
      const points = new THREE.Points(geometry, material); //点模型对象
      scene.add(points); //点对象添加到场

      const axisHelper = new THREE.AxesHelper(250);
      scene.add(axisHelper);

      const ambient = new THREE.AmbientLight(0x444444);
      scene.add(ambient);

      //点光源
      const point = new THREE.PointLight(0xffffff);
      point.position.set(300, 200, 400); //点光源位置
      scene.add(point); //点光源添加到场景中

      const width = window.innerWidth;
      const height = window.innerHeight;
      const k = width / height;
      const s = 300;

      const camera = new THREE.OrthographicCamera(-s * k, s * k, s, -s, 1, 1000);
      camera.position.set(200, 250, 300);
      camera.lookAt(scene.position);

      const renderer = new THREE.WebGLRenderer();
      renderer.setSize(width, height);
      renderer.setClearColor(0xb9d3ff, 1);

      document.getElementById('threeCanvas').appendChild(renderer.domElement);

      function render() {
        renderer.render(scene,camera);//执行渲染操作
      }

      render();

      const controls = new OrbitControls( camera, renderer.domElement );

      controls.addEventListener('change', render);
    },

  },
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style>
html, body {
  padding: 0;
  margin: 0;
}
#three {
  width: 100%;
  height: 100%;
  position: fixed;
  left: 0;
  top: 0;
}
</style>
