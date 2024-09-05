<script>
  import { getContext } from "svelte"
  import JsBarcode from "jsbarcode"
  import { onMount, afterUpdate } from "svelte"


  export let value
  export let QRCode
  export let showValue

  const { styleable } = getContext("sdk")
  const component = getContext("component")

  let barcode; 

  const generateBarcode = () => {
    if (value) {
      JsBarcode("#barcode", value, {
        displayValue: showValue,
      });
    }
  };

  onMount(() => {
    generateBarcode();
  });

  afterUpdate(() => {
    generateBarcode();
  });




</script>

<div use:styleable={$component.styles}>
  Value = {value}.
  QRCode = {QRCode}.
  showValue = {showValue}
  
  <img id="barcode" alt="barcode {showValue ? value : ""}"/>
  
</div>
