# 3D Configurator — Threlte + SvelteKit

Progetto didattico per mostrare come costruire un configuratore 3D con **SvelteKit**, **Threlte** e **Three.js**.

## Setup del progetto

### 1. Crea il progetto SvelteKit

```sh
npx sv create . --template minimal --types ts --no-install
```

### 2. Installa le librerie Threlte e Three.js

```sh
# Runtime
npm install three @threlte/core @threlte/extras

# Dev: tipi TypeScript + Threlte Studio (debug visuale)
npm install -D @types/three @threlte/studio

# Dev: necessario per @threlte/gltf (formattazione output)
npm install -D prettier prettier-plugin-svelte
```

### 3. Aggiungi il plugin Vite per Threlte Studio

In `vite.config.ts`:

```ts
import { sveltekit } from '@sveltejs/kit/vite'
import { defineConfig } from 'vite'
import { threlteStudio } from '@threlte/studio/vite'

export default defineConfig({
  plugins: [
    threlteStudio({ apply: 'serve' }), // solo in development
    sveltekit()
  ]
})
```

### 4. Disabilita SSR (inutile per app WebGL)

Crea `src/routes/+layout.ts`:

```ts
export const ssr = false
```

---

## Aggiungere un modello 3D custom

### Step 1 — Copia il file `.glb`

Metti il file nella cartella `static/models/`:

```
static/
  models/
    tuomodello.glb
```

> I file in `static/` sono serviti dalla root del sito: `tuomodello.glb` sarà accessibile su `/models/tuomodello.glb`.

### Step 2 — Converti il modello in componente Svelte

```sh
npx @threlte/gltf@next static/models/tuomodello.glb \
  --output src/lib/components/TuoModello.svelte \
  --types \
  --shadows
```

Il comando genera un componente con tutti i nodi e materiali già nominati, pronti da usare.

> **Nota:** `@threlte/gltf` scrive il path come `/static/models/...` ma in SvelteKit è sbagliato.
> Apri il file generato e correggi la riga `useGltf(...)`:
> ```ts
> // ❌ errato (generato automaticamente)
> const gltf = useGltf('/static/models/tuomodello.glb')
>
> // ✅ corretto
> const gltf = useGltf('/models/tuomodello.glb')
> ```

### Step 3 — Collega il materiale ai controlli

Nel componente generato, importa `config` e aggiungi un `$effect`:

```svelte
<script lang="ts">
  import { config } from '$lib/config.svelte.js'
  // ... resto degli import generati ...

  const gltf = useGltf('/models/tuomodello.glb')

  // Aggiorna il materiale ogni volta che config cambia
  $effect(() => {
    if (!$gltf) return
    const mat = $gltf.materials.ilTuoMateriale
    mat.color.set(config.color)
    mat.roughness = config.roughness
    mat.metalness = config.metalness
  })
</script>
```

### Step 4 — Usalo nella scena

In `src/lib/components/Scene.svelte`, sostituisci il modello precedente:

```svelte
<script>
  import TuoModello from './TuoModello.svelte'
</script>

<!-- ... luci e camera ... -->
<TuoModello />
```

---

## Sviluppo

```sh
npm run dev          # avvia il server (Threlte Studio attivo)
npm run dev -- --open  # apre anche il browser
```

## Build

```sh
npm run build        # build di produzione (Studio escluso)
npm run preview      # anteprima del build
```
