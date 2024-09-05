<script>
  import { getContext } from "svelte";
  import { onMount, afterUpdate } from "svelte";
  import JsBarcode from "jsbarcode";
	import { createQrSvgString, createQrSvgDataUrl } from '@svelte-put/qr';
  
  export let value;
  export let showQR = true;
  export let showValue;

  const { styleable } = getContext("sdk");
  const component = getContext("component");

  // BARCODES
  let barcode;
  let qrContainer;

  const generateBarcode = () => {
    if (value) {
      JsBarcode("#barcode", value, {
        displayValue: showValue,
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
      {@html createQrSvgString({data: value, moduleFill: "black", anchorOuterFill: "black", anchorInnerFill: "black", width: 200, height: 200})}
    </div>
    {:else}
      <img id="barcode" alt="barcode {value ? value : ""}"/>
    {/if}
  {:else}
    <p>Please add a value to generate your {showQR ? "QR Code" : "Barcode"}</p>
  {/if}



  
</div>
