<script>
  import { getContext } from "svelte"
  import JsBarcode from "jsbarcode"
  import { onMount, afterUpdate } from "svelte"


  export let value
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
  <img id="barcode" alt="barcode {showValue ? value : ""}"/>
</div>
