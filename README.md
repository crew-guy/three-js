3 basic things required to start any 3 js project :

1. Scene
2. Camera
3. Renderer

2 basic things required to setup any object in 3 js :

1. Geometry
2. Material

## Standard sample stuff

1. INITIATING A PROJECT WITH - scene, camera, renderer

    ```jsx
    import * as THREE from 'three'

    //! INITIATING A PROJECT WITH - scene, camera, renderer
    const scene = new THREE.Scene()

    const camera = new THREE.PerspectiveCamera(
      75,
      window.innerWidth / window.innerHeight,
      0.1,
      1000
    )

    const renderer = new THREE.WebGLRenderer({
      canvas : document.querySelector('#bg')
    })

    renderer.setPixelRatio(window.devicePixelRatio)
    renderer.setSize(window.innerWidth, window.innerHeight)
    camera.position.setZ(20)

    renderer.render(scene, camera)
    ```

2. Adding an **object**

    ```jsx
    //! ADDING A NEW OBJECT
    const geometry = new THREE.TorusGeometry(10, 3, 16, 100)
    // const material = new THREE.MeshBasicMaterial({ color: 0xFF6347, wireframe: true })
    const material = new THREE.MeshStandardMaterial({ color: 0xFF6347 })
    const torus = new THREE.Mesh(geometry, material)

    scene.add(torus)
    ```

3. Working with **lights, light helpers** and **grid lines**

    ```jsx
    //! WORKING WITH LIGHTS, LIGHT HELPERS AND GRID LINES
    const pointLight = new THREE.PointLight(0xffffff)
    pointLight.position.set(20, 5, 5)

    const ambientLight = new THREE.AmbientLight(0xffffff)

    // scene.add(pointLight)
    // scene.add(ambientLight)
    scene.add(pointLight, ambientLight)

    const lightHelper = new THREE.PointLightHelper(pointLight)
    const gridHelper = new THREE.GridHelper(200,50)
    scene.add(lightHelper, gridHelper)

    // Listen to DOM events on mouse and update camera position accordingly
    const controls = new OrbitControls(camera, renderer.domElement)
    ```

4. **Animating** an object

    ```jsx
    //! ANIMATING AN OBJECT (principle of recursion used)
    const animate = () => {
      requestAnimationFrame(animate)

      torus.rotation.x += 0.01
      torus.rotation.y += 0.005
      torus.rotation.z += 0.01

      controls.update()
      renderer.render(scene, camera)
    }

    animate()
    ```

5. **Randomly populating space with multiple objects**

    ```jsx
    //! RANDOMLY POPULATE THE SPACE WITH A LARGE NUMBER OF OBJECTS
    function addStar() {
      const geometry = new THREE.SphereGeometry(0.25, 24, 24)
      const material = new THREE.MeshStandardMaterial({ color: 0xffffff })
      const star = new THREE.Mesh(geometry, material)

      const [x,y,z] = Array(3).fill().map(() => THREE.MathUtils.randFloatSpread(100))

      star.position.set(x, y, z)
      scene.add(star)
    }
    Array(200).fill().forEach(addStar)
    ```

6. **Images, Texture mapping** and **Depth Maps**

    ```jsx
    //! LOADING IMAGES USING TEXTURE
    const spaceTexture = new THREE.TextureLoader().load('./stars.jpg')
    scene.background = spaceTexture

    //! CUSTOM SURFACE OBJECTS USING TEXTURE MAPPING
    const ankitTexture = new THREE.TextureLoader().load('./ankit-profile.png')

    const ankit = new THREE.Mesh (
      new THREE.BoxGeometry(3, 3, 3),
      new THREE.MeshStandardMaterial({map : ankitTexture})
    )

    scene.add(ankit) 
    ****
    //! USING DEPTH MAP
    const moonTexture = new THREE.TextureLoader().load('./moon.jpg');
    const normalTexture = new THREE.TextureLoader().load('./purple.jpg')

    const moon = new THREE.Mesh(
      new THREE.SphereGeometry(3,32,32),
      new THREE.MeshStandardMaterial({map:moonTexture, normalMap:normalTexture})
    )
    moon.position.z = 10
    moon.position.setX(-9)

    scene.add(moon)
    ```

7. Moving camera **on scroll**

    ```jsx
    const moveCamera = () => {
      const t = document.body.getBoundingClientRect().top

      moon.rotation.x += 0.05
      moon.rotation.y += 0.75
      moon.rotation.z += 0.05

      jeff.rotation.y += 0.01;
      jeff.rotation.z += 0.01;

      camera.position.z = t*(-0.01)
      
    }

    document.body.onScroll = moveCamera
    ```
