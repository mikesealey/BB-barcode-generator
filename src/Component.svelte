<script>
  import { getContext } from "svelte";
  import { onMount, afterUpdate } from "svelte";
  import JsBarcode from "jsbarcode";
	import { createQrSvgString, createQrSvgDataUrl } from '@svelte-put/qr';
  
  export let value;
  export let showQR;
  export let showValue;
  export let customLogo;
  export let size;
  export let primColor;
  export let secColor;

  const { styleable } = getContext("sdk");
  const component = getContext("component");

  // BARCODES
  let barcode;
  let qrContainer;

  const generateBarcode = () => {
    let barcodeSize = size / 100
    if (value) {
      JsBarcode("#barcode", value, {
        displayValue: showValue,
        width: barcodeSize,
        lineColor: primColor,
        background: secColor
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
  <p>{primColor} {secColor}</p>
  {#if value}
    {#if showQR}
    <div>
      {@html createQrSvgString({
        data: value, logo: customLogo, 
        moduleFill: primColor, 
        anchorOuterFill: primColor, 
        anchorInnerFill: primColor, 
        backgroundFill: secColor, 
        width: size, 
        height: size
        })}
      <div style="word-wrap: break-word; overflow-wrap: break-word; width: {size};">{showValue ? value : ""}</div>
    </div>
    {:else}
    <div >
      <img id="barcode" alt="barcode {value ? value : ""}"/>
    </div>
    {/if}
  {:else}
    <p>Please add a value to generate your {showQR ? "QR Code" : "Barcode"}</p>
  {/if}



  
</div>
