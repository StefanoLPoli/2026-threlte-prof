<script>
  import { Canvas } from '@threlte/core'
  import Scene from '$lib/components/Scene.svelte'
  
  // Dati placeholder - le rotazioni sono in radianti
  const faces = [
    { 
      name: 'Maksym Halinichev', 
      rotation: { x: 0, y: 0, z: 0 },
      position: { x: 0.0, y: 0.0, z: 0 }
    },
    { 
      name: 'Dmytro Sharpar', 
      rotation: { x: 0, y: 1.571, z: 0 },
      position: { x: 0, y: 0.0, z: 0 } 
    },
    { 
      name: 'Yevhen Malyshev', 
      rotation: { x: 0, y: 1.0, z: 0 },
      position: { x: 0, y: 0, z: 0 }
    },
    { 
      name: 'Karyna Bakhur', 
      rotation: { x: 0, y: 1.5, z: 0 },
      position: { x: 0, y: 0, z: 0}
    },
    { 
      name: 'Taras Shpuk', 
      rotation: { x: 0, y: 2.0, z: 0 },
      position: { x: 0, y: 0, z: 0 }
    },
    { 
      name: 'Pavlo Ishchenko', 
      rotation: { x: 0, y: 2.5, z: 0 },
      position: { x: 0, y: 0, z: 0 }
    },
    { 
      name: 'Oleksiy Loginov', 
      rotation: { x: 0, y: 3.0, z: 0 },
      position: { x: 0, y: 0, z: 0 }
    },
    { 
      name: 'Volodymyr Androshchuk', 
      rotation: { x: 0, y: 3.5, z: 0 },
      position: { x: 0, y: 0, z: 0 }
    },
    { 
      name: 'Oleksiy Khabarov', 
      rotation: { x: 0, y: 4.0, z: 0 },
      position: { x: 0, y: 0, z: 0 }
    },
    { 
      name: 'Mykyta Kozubenko', 
      rotation: { x: 0, y: 4.5, z: 0 },
      position: { x: 0, y: 0, z: 0 }
    },
    { 
      name: 'Andriy Yaremenko', 
      rotation: { x: 0, y: 5.0, z: 0 },
      position: { x: 0, y: 0, z: 0 }
    },
    { 
      name: 'Fedor Epifanov', 
      rotation: { x: 0, y: 5.5, z: 0 },
      position: { x: 0, y: 0, z: 0 }
    },
    { 
      name: 'Kateryna Troyan', 
      rotation: { x: 0, y: 6.0, z: 0 },
      position: { x: 0, y: 0, z: 0 }
    },
    { 
      name: 'Nazar Zuy', 
      rotation: { x: 0, y: 6.5, z: 0 },
      position: { x: 0, y: 0, z: 0 }
    },
    { 
      name: 'Mariia Lebid', 
      rotation: { x: 0, y: 7.0, z: 0 },
      position: { x: 0, y: 0, z: 0 }
    },
    { 
      name: 'Alina Perehudova', 
      rotation: { x: 0, y: 7.5, z: 0 },
      position: { x: 0, y: 0, z: 0 }
    },
    { 
      name: 'Oleksandr Peleshenko', 
      rotation: { x: 0, y: 8.0, z: 0 },
      position: { x: 0, y: 0, z: 0 }
    },
    { 
      name: 'Kateryna Diachenko', 
      rotation: { x: 0, y: 8.5, z: 0 },
      position: { x: 0, y: 0, z: 0 }
    },
    { 
      name: 'Viktoriia Ivashko', 
      rotation: { x: 0, y: 9.0, z: 0 },
      position: { x: 0, y: 0, z: 0 }
    },
    { 
      name: 'Andriy Kutsenko', 
      rotation: { x: 0, y: 9.5, z: 0 },
      position: { x: 0, y: 0, z: 0 }
    },
    { 
      name: 'Daria Kurdel', 
      rotation: { x: 0, y: 10, z: 0 },
      position: { x: 0, y: 0, z: 0 }
    },
    { 
      name: 'Ivan Kononenko', 
      rotation: { x: 0, y: 10.5, z: 0 },
      position: { x: 0, y: 0, z: 0 }
    }
  ];
  let selected = $state(0);
  let activeRotation = $state(faces[0].rotation);
  let activePosition = $state(faces[0].position);
  let timeoutId = null;
  let visibleStart = $state(0);
  
  // Funzione per aggiornare la rotazione con delay
  function updateRotationWithDelay(index) {
    // Cancella il timeout precedente se esiste
    if (timeoutId) {
      clearTimeout(timeoutId);
    }
    
    // Imposta un nuovo timeout di 1 secondo
    timeoutId = setTimeout(() => {
      activeRotation = faces[index].rotation;
      activePosition = faces[index].position;
    }, 400);
  }
  
  $effect(() => {
    if (selected < 2) {
      visibleStart = 0;
    } else if (selected > faces.length - 3) {
      visibleStart = Math.max(0, faces.length - 5);
    } else {
      visibleStart = selected - 2;
    }
  });
  
  function getVisibleFaces() {
    const end = Math.min(visibleStart + 5, faces.length);
    return faces.slice(visibleStart, end);
  }
  
  function onwheel(e) {
    e.preventDefault();
    if (e.deltaY > 0 && selected < faces.length - 1) {
      selected++;
    } else if (e.deltaY < 0 && selected > 0) {
      selected--;
    }
    // Chiama la funzione con delay ogni volta che cambia la selezione
    updateRotationWithDelay(selected);
  }
  
  function selectFace(index) {
    selected = visibleStart + index;
    updateRotationWithDelay(selected);
  }
</script>

<div class="gallery-layout" onwheel={onwheel}>
  <div class="names">
    {#each getVisibleFaces() as face, i}
      {@const actualIndex = visibleStart + i}
      <div 
        class="name {actualIndex === selected ? 'selected' : ''}"
        onclick={() => selectFace(i)}
        role="button"
        tabindex="0"
        onkeydown={(e) => e.key === 'Enter' && selectFace(i)}
      >
        {face.name}
      </div>
    {/each}
  </div>
  
  <div class="viewer">
    <Canvas renderMode="always">
      <Scene 
        rotation={activeRotation}
        position={activePosition}
        orbitControlVal={false}
      />
    </Canvas>
  </div>
</div>

<style>
  .gallery-layout {
    display: flex;
    height: 100vh;
    width: 100vw;
    background: #000000;
    position: relative;
  }
  
  .names {
    position: absolute;
    left: 0;
    top: 0;
    height: 100vh;
    width: 100%;
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: flex-start;
    gap: 3rem;
    padding: 0 5rem;
    user-select: none;
    z-index: 10;
    pointer-events: none;
  }
  
  .name {
    font-size: 5rem;
    font-weight: 900;
    color: rgba(255, 255, 255, 0.1);
    transition: all 0.5s cubic-bezier(0.4, 0, 0.2, 1);
    cursor: pointer;
    letter-spacing: -0.03em;
    line-height: 1;
    pointer-events: auto;
    text-transform: uppercase;
  }
  
  .name.selected {
    color: rgba(255, 255, 255, 1);
    transform: scale(1.15);
    text-shadow: 
      0 0 20px rgba(255, 255, 255, 0.5),
      0 0 40px rgba(255, 255, 255, 0.3),
      0 0 80px rgba(255, 255, 255, 0.2);
    margin-left: 2rem;
  }
  
  .name:hover {
    color: rgba(255, 255, 255, 0.4);
    transform: scale(1.05);
    margin-left: 1rem;
  }
  
  .name.selected:hover {
    color: rgba(255, 255, 255, 1);
    transform: scale(1.15);
  }
  
  .viewer {
    position: absolute;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    background: #000000;
    display: flex;
    align-items: center;
    justify-content: flex-end;
    padding-left: 45%;
  }
  

</style>