<script>
  import { Canvas } from '@threlte/core'
  import Scene from '$lib/components/Scene.svelte'
  
  // Dati degli atleti - le rotazioni sono in radianti
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
  
  function updateRotationWithDelay(index) {
    if (timeoutId) {
      clearTimeout(timeoutId);
    }
    
    timeoutId = setTimeout(() => {
      activeRotation = faces[index].rotation;
      activePosition = faces[index].position;
    }, 400);
  }
  
  // Calcola i 5 slot (sempre 5, con eventuali spazi vuoti)
  let displaySlots = $derived.by(() => {
    const slots = [];
    
    // 2 slot sopra il selezionato
    for (let i = 2; i >= 1; i--) {
      const index = selected - i;
      if (index >= 0) {
        slots.push({ name: faces[index].name, index: index, isEmpty: false });
      } else {
        slots.push({ name: '', index: -1, isEmpty: true });
      }
    }
    
    // Il selezionato al centro
    slots.push({ name: faces[selected].name, index: selected, isEmpty: false });
    
    // 2 slot sotto il selezionato
    for (let i = 1; i <= 2; i++) {
      const index = selected + i;
      if (index < faces.length) {
        slots.push({ name: faces[index].name, index: index, isEmpty: false });
      } else {
        slots.push({ name: '', index: -1, isEmpty: true });
      }
    }
    
    return slots;
  });
  
  function onwheel(e) {
    e.preventDefault();
    if (e.deltaY > 0 && selected < faces.length - 1) {
      selected++;
    } else if (e.deltaY < 0 && selected > 0) {
      selected--;
    }
    updateRotationWithDelay(selected);
  }
  
  function selectFace(index) {
    if (index !== -1) {
      selected = index;
      updateRotationWithDelay(selected);
    }
  }
</script>

<div class="gallery-layout" onwheel={onwheel}>
  <div class="names">
    {#each displaySlots as slot}
      {#if slot.isEmpty}
        <div class="name empty"></div>
      {:else}
        <div 
          class="name {slot.index === selected ? 'selected' : ''}"
          onclick={() => selectFace(slot.index)}
          role="button"
          tabindex="0"
          onkeydown={(e) => e.key === 'Enter' && selectFace(slot.index)}
        >
          {slot.name}
        </div>
      {/if}
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
  @font-face {
    font-family: 'GeistPixel';
    src: url('/fonts/GeistPixel-Square.woff2') format('woff2'),
        url('/fonts/GeistPixel-Square.woff') format('woff'),
        url('/fonts/GeistPixel-Square.ttf') format('truetype');
    font-weight: normal;
    font-style: normal;
  }

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
    font-family: 'GeistPixel', monospace;
    font-size: 5rem;
    font-weight: normal;
    color: transparent;
    -webkit-text-stroke: 1.5px rgba(255, 255, 255, 0.5);
    transition: all 0.5s cubic-bezier(0.4, 0, 0.2, 1);
    cursor: pointer;
    letter-spacing: -0.03em;
    line-height: 1;
    pointer-events: auto;
    text-transform: uppercase;
  }
  
  .name.empty {
    pointer-events: none;
  }

  .name.selected {
    font-family: 'GeistPixel', monospace;
    color: rgba(255, 255, 255, 1);
    -webkit-text-stroke: 0;
    transform: scale(1.30);
    transform-origin: left center;
    text-shadow: 
      0 0 20px rgba(255, 255, 255, 0.5),
      0 0 40px rgba(255, 255, 255, 0.3),
      0 0 80px rgba(255, 255, 255, 0.2);
  }
  
  .name:hover:not(.empty) {
    -webkit-text-stroke: 1.5px rgba(255, 255, 255, 0.9);
    
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
