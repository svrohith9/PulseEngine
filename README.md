```markdown
# PulseEngine: A Real-Time Audio Visualizer

Welcome to **PulseEngine**, an interactive web application that transforms audio from your microphone into captivating, real-time art. This project uses the Web Audio API to analyze sound frequencies and generate dynamic visuals that react to music, voice, or any other audio input.

---

### How to Use

1.  **Open the Application:** Open the `index.html` file in your browser, or visit the live version hosted on GitHub Pages.
2.  **Grant Permission:** Click the **"Start Listening"** button. Your browser will ask for permission to access your microphone. You must click **"Allow"** for the visualizer to work.
3.  **Play Audio:** Play music or speak into your microphone.
4.  **Switch Modes:** Use the buttons at the top of the screen to switch between different visualization styles.

---

### Features

The application comes with three unique, pre-built visualizer modes:

* **Cosmic Orb:** A central, pulsing orb surrounded by a spiral of particles that dance to the music's frequencies. Strong bass hits create radiating light rays for a powerful effect.
* **Retro Bars:** A classic audio spectrum visualizer. Colorful bars rise and fall based on the volume of different frequency bands, creating a nostalgic, equalizer-like display.
* **GenZ Glitch:** A modern, chaotic visualizer that generates random shapes and flashes across the screen. Strong bass triggers a "screen shake" effect for an energetic, glitchy feel.

---

### Customization & How to Add Your Own Visuals

PulseEngine is designed to be easily customizable. You can tweak the existing modes or add entirely new ones by editing the `index.html` file.

#### **1. Finding the Code**

All the visualization logic is located within the `<script>` tag at the bottom of the `index.html` file. Each mode has its own dedicated drawing function:

* `drawCosmicOrb()`
* `drawRetroBars()`
* `drawGenzGlitch()`

#### **2. How to Add a New Mode**

Let's say you want to create a new mode called "Waveform".

**Step A: Create a New Button**
In the HTML, add a button for your new mode inside the `mode-selector` div:

```html
<button id="mode-waveform" class="mode-button text-gray-300 ...">Waveform</button>
```

**Step B: Create a New Drawing Function**
In the JavaScript section, create a new function called `drawWaveform()`. You can use the `dataArray` (which holds the audio data) to create your visuals.

```javascript
// 4. Your New Waveform Mode
function drawWaveform() {
    // Set a background color
    ctx.fillStyle = 'rgba(20, 20, 30, 0.1)';
    ctx.fillRect(0, 0, canvas.width, canvas.height);

    // Set the line color and width
    ctx.lineWidth = 3;
    ctx.strokeStyle = `hsl(${hue}, 100%, 70%)`;
    ctx.beginPath();

    const sliceWidth = canvas.width * 1.0 / bufferLength;
    let x = 0;

    for (let i = 0; i < bufferLength; i++) {
        const v = dataArray[i] / 128.0; // Normalize data
        const y = v * canvas.height / 2;

        if (i === 0) {
            ctx.moveTo(x, y);
        } else {
            ctx.lineTo(x, y);
        }
        x += sliceWidth;
    }

    ctx.lineTo(canvas.width, canvas.height / 2);
    ctx.stroke();
}
```

**Step C: Connect the Function in the Draw Loop**
Finally, add a `case` for your new mode in the `switch` statement inside the main `draw()` function:

```javascript
switch(currentMode) {
    case 'cosmic':
        drawCosmicOrb();
        break;
    case 'retro':
        drawRetroBars();
        break;
    case 'genz':
        drawGenzGlitch();
        break;
    case 'waveform': // Add this new case
        drawWaveform();
        break;
}
```

Now, your new "Waveform" mode is fully integrated!

#### **3. Tweaking Existing Visuals**

Feel free to experiment by changing the values inside the drawing functions. You can easily adjust:

* **Colors:** Modify the `hsl()` or `hsla()` color values. Changing `hue` will produce different color schemes.
* **Sizes & Shapes:** Change values related to `radius`, `particleSize`, or `barWidth`.
* **Sensitivity:** In the `initAudio()` function, change `analyser.fftSize` to a different power of 2 (e.g., `256` for faster but less detailed visuals, or `1024` for more detail). You can also change the conditions in `if` statements (e.g., `if (bassValue > 180)`).
```
