import { loadGLTF, loadAudio } from "../../libs/loader.js";
const THREE = window.MINDAR.IMAGE.THREE;

document.addEventListener('DOMContentLoaded', () => {
  const start = async () => {
    // Initialize MindARThree
    const mindarThree = new window.MINDAR.IMAGE.MindARThree({
      container: document.body,
      imageTargetSrc: '../../assets/targets/storybook/dino.mind',
    });

    const { renderer, scene, camera } = mindarThree;

    // Add a light source
    const light = new THREE.HemisphereLight(0xffffff, 0xbbbbff, 1);
    scene.add(light);

    // Clock for animations
    const clock = new THREE.Clock();

    // Load and configure pages (models, audio, animations)
    const pages = [
      { modelPath: '../../assets/models/g20/page1.glb', audioPath: '../../assets/audio/BM/scene1.mp3', scale: [0.3, 0.3, 0.3], position: [0.0, -0.4, 0], rotation: [0, 0, 0] },
      { modelPath: '../../assets/models/g20/page2.glb', audioPath: '../../assets/audio/BM/scene2.mp3', scale: [0.4, 0.4, 0.4], position: [0.0, -0.4, 0], rotation: [0, 0, 0] },
	   { modelPath: '../../assets/models/g20/page3.glb', audioPath: '../../assets/audio/BM/scene3.mp3', scale: [0.4, 0.4, 0.4], position: [0.0, -0.4, 0], rotation: [0, 0, 0] },
      { modelPath: '../../assets/models/g20/page4.glb', audioPath: '../../assets/audio/BM/scene4.mp3', scale: [0.4, 0.4, 0.4], position: [0.0, -0.4, 0], rotation: [0, 0, 0] },
	  { modelPath: '../../assets/models/g20/page5.glb', audioPath: '../../assets/audio/BM/scene5.mp3', scale: [0.4, 0.4, 0.4], position: [0.0, -0.4, 0], rotation: [0, 0, 0] },
	   { modelPath: '../../assets/models/g20/page6.glb', audioPath: '../../assets/audio/BM/scene6.mp3', scale: [0.4, 0.4, 0.4], position: [0.0, -0.4, 0], rotation: [0, 0, 0] },
       { modelPath: '../../assets/models/g20/page7.glb', audioPath: '../../assets/audio/BM/scene7.mp3', scale: [0.3, 0.3, 0.3], position: [0.0, -0.4, 0], rotation: [0, 0, 0] },
      { modelPath: '../../assets/models/g20/page8.glb', audioPath: '../../assets/audio/BM/scene8.mp3', scale: [0.5, 0.5, 0.5], position: [0.0, -0.4, 0], rotation: [0, 0, 0] },
	  { modelPath: '../../assets/models/g20/page9.glb', audioPath: '../../assets/audio/BM/scene9.mp3', scale: [0.7, 0.7, 0.7], position: [0.0, -0.4, 0], rotation: [0, 0, 0] },
       { modelPath: '../../assets/models/g20/page10.glb', audioPath: '../../assets/audio/BM/scene10.mp3', scale: [0.7, 0.7, 0.7], position: [0.0, -0.4, 0], rotation: [0, 0, 0] },
      { modelPath: '../../assets/models/g20/page11.glb', audioPath: '../../assets/audio/BM/scene11.mp3', scale: [0.7, 0.7, 0.7], position: [0.0, -0.4, 0], rotation: [0, 0, 0] },
	 
	  // Add other pages here
    ];

    const mixers = [];

    for (let i = 0; i < pages.length; i++) {
      const { modelPath, audioPath, scale, position, rotation } = pages[i];
      try {
        const gltf = await loadGLTF(modelPath);
        gltf.scene.scale.set(...scale);
        gltf.scene.position.set(...position);
        gltf.scene.rotation.set(...rotation);

        const anchor = mindarThree.addAnchor(i);
        anchor.group.add(gltf.scene);

        const audioClip = await loadAudio(audioPath);
        const listener = new THREE.AudioListener();
        camera.add(listener);

        const positionalAudio = new THREE.PositionalAudio(listener);
        positionalAudio.setBuffer(audioClip);
        positionalAudio.setRefDistance(10000);
        positionalAudio.setLoop(true);
        anchor.group.add(positionalAudio);

        anchor.onTargetFound = () => {
          positionalAudio.play();
        };
        anchor.onTargetLost = () => {
          positionalAudio.stop();
        };

        const mixer = new THREE.AnimationMixer(gltf.scene);
        if (gltf.animations.length > 0) {
          const action = mixer.clipAction(gltf.animations[0]);
          action.play();
        }
        mixers.push(mixer);
      } catch (error) {
        console.error(`Error loading page ${i + 1}:`, error);
      }
    }

    // Start MindAR and render loop
    try {
      await mindarThree.start();
      renderer.setAnimationLoop(() => {
        const delta = clock.getDelta();
        mixers.forEach(mixer => mixer.update(delta));
        renderer.render(scene, camera);
      });
    } catch (error) {
      console.error("Error starting MindAR:", error);
    }
  };

  start();
});
