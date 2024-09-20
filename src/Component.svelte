<script>
  import { getContext } from "svelte";
  import { onMount, afterUpdate } from "svelte";
  import JsBarcode from "jsbarcode";
	import { createQrSvgString, createQrSvgDataUrl } from '@svelte-put/qr';
  import '@spectrum-css/vars/dist/spectrum-global.css';

  export let value;
  export let showQR;
  export let showValue;
  export let customLogo;
  export let size;
  export let primColor;

  const { styleable } = getContext("sdk");
  const component = getContext("component");

  // BARCODES
  let barcode;
  let qrContainer;
  let convertedColour;

  const generateBarcode = () => {
    let barcodeSize = size / 100
    if (value) {
      JsBarcode("#barcode", value, {
        displayValue: showValue,
        width: barcodeSize,
        height: size,
        lineColor: "black",
        background: "white"
      });
    }
  };

  onMount(() => {
    if (showQR) {
  // Generate QR Code
  console.log("generating QR code")
    } else {
      generateBarcode();
    }
  });

  afterUpdate(() => {
    if (showQR) {
// Generate QR code
console.log("generating QR code")
    } else {
      generateBarcode();
    }
  });

</script>

<div class="overall" use:styleable={$component.styles}>
  {#if value}
    {#if showQR}
    <div>
      {@html createQrSvgString({
        data: value, logo: customLogo, 
        moduleFill: primColor, 
        anchorOuterFill: primColor, 
        anchorInnerFill: primColor, 
        width: size, 
        height: size
        })}
      <div style="word-wrap: break-word; overflow-wrap: break-word; width: {size}; text-align: center;">{showValue ? value : ""}</div>
    </div>
    {:else}
    <div class="barcode-container">
      {#if customLogo}
        <img src={customLogo} alt="logo" class="barcode-logo" style="height: {size}px;" />
      {/if}
      <img id="barcode" alt="barcode {value ? value : ""}" class="barcode-image"/>
    </div>
    {/if}
  {:else}
    <p>Please add a value to generate your {showQR ? "QR Code" : "Barcode"}</p>
  {/if}
</div>

<style>
.barcode-container {
  display: flex;
  align-items: center;
}

</style>