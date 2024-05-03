<script lang="ts">
let baseImage: HTMLImageElement;
let blendImage: HTMLImageElement;
let blendedImageDataUrl: string;

function softLight(base: number, blend: number): number {
	const d = (c: number) =>
		c <= 0.25 ? (16 * c * c - 12) * c + 4 : Math.sqrt(c);
	if (blend <= 0.5) {
		return base * (1 - 2 * blend) + d(blend) * (2 * blend - 1);
	}
	return base + (1 - base) * Math.sqrt(blend);
}

function multiply(base: number, blend: number): number {
	return base * blend;
}

function screen(base: number, blend: number): number {
	return 1 - (1 - base) * (1 - blend);
}

function overlay(base: number, blend: number): number {
	return base < 0.5 ? 2 * base * blend : 1 - 2 * (1 - base) * (1 - blend);
}

function darken(base: number, blend: number): number {
	return Math.min(base, blend);
}

function lighten(base: number, blend: number): number {
	return Math.max(base, blend);
}

function colorDodge(base: number, blend: number): number {
	return blend === 1 ? 1 : Math.min(base / (1 - blend), 1);
}

function colorBurn(base: number, blend: number): number {
	return blend === 0 ? 0 : 1 - Math.min((1 - base) / blend, 1);
}

function blendImages(
	blendModeFunction: (base: number, blend: number) => number,
) {
	console.log("Blending images");
	const baseCanvas = document.createElement("canvas");
	const blendCanvas = document.createElement("canvas");
	const blendedCanvas = document.createElement("canvas");
	const baseCtx = baseCanvas.getContext("2d");
	const blendCtx = blendCanvas.getContext("2d");
	const blendedCtx = blendedCanvas.getContext("2d");

	baseCanvas.width = baseImage.width;
	baseCanvas.height = baseImage.height;
	blendCanvas.width = blendImage.width;
	blendCanvas.height = blendImage.height;
	blendedCanvas.width = baseImage.width;
	blendedCanvas.height = baseImage.height;

	if (baseCtx) {
		baseCtx.drawImage(baseImage, 0, 0);
	}
	if (blendCtx) {
		blendCtx.drawImage(blendImage, 0, 0);
	}

	const baseData = baseCtx?.getImageData(
		0,
		0,
		baseCanvas.width,
		baseCanvas.height,
	);
	const blendData = blendCtx?.getImageData(
		0,
		0,
		blendCanvas.width,
		blendCanvas.height,
	);
	const blendedData = blendedCtx?.createImageData(
		baseCanvas.width,
		baseCanvas.height,
	);

	if (!baseData || !blendData || !blendedData) {
		return;
	}

	for (let i = 0; i < baseData.data.length; i += 4) {
		for (let j = 0; j < 3; j++) {
			// Apply the blend mode function to R, G, B channels
			const baseValue = baseData.data[i + j] / 255;
			const blendValue = blendData.data[i + j] / 255;
			const blendedValue = blendModeFunction(baseValue, blendValue);
			blendedData.data[i + j] = blendedValue * 255;
		}
		blendedData.data[i + 3] = 255; // Set alpha channel to 255
	}

	if (blendedCtx) {
		blendedCtx.putImageData(blendedData, 0, 0);
	}
	blendedImageDataUrl = blendedCanvas.toDataURL();

	console.log("Images blended using the provided blend mode function");
}

// Helper function to handle blend mode selection and blending
function blendWithMode(modeFunction: (base: number, blend: number) => number) {
	if (baseImage.complete && blendImage.complete) {
		blendImages(modeFunction);
	}
}
</script>

<h1>Blend modes</h1>
{#if blendedImageDataUrl}
    <h2>Blended Image</h2>
    <img src="{blendedImageDataUrl}" alt="Blended Image" />
{/if}

<img src="base.png" alt="Base Image" bind:this={baseImage}  />
<img src="blend.png" alt="Blend Image" bind:this={blendImage}  />

<button on:click={() => blendWithMode(softLight)}>Soft Light</button>
<button on:click={() => blendWithMode(multiply)}>Multiply</button>
<button on:click={() => blendWithMode(screen)}>Screen</button>
<button on:click={() => blendWithMode(overlay)}>Overlay</button>
<button on:click={() => blendWithMode(darken)}>Darken</button>
<button on:click={() => blendWithMode(lighten)}>Lighten</button>
<button on:click={() => blendWithMode(colorDodge)}>Color Dodge</button>
<button on:click={() => blendWithMode(colorBurn)}>Color Burn</button>
<button on:click={() => {
    const a = document.createElement('a');
    a.href = blendedImageDataUrl;
    a.download = 'blended.png';
    a.click();
}}>Download Blended Image</button>