<script lang="ts">
  import { OctagonAlert, SaveIcon } from "@lucide/svelte";
  import { alertMd } from "src/ts/alert";
  import { saving } from "src/ts/globalApi.svelte";
  import { AccountWarning } from "src/ts/storage/accountStorage";
  import { DBState } from "src/ts/stores.svelte";

  const clampPercent = (value: number) => {
    if (!Number.isFinite(value)) {
      return 0;
    }
    return Math.max(0, Math.min(100, value));
  };
</script>

{#if DBState?.db?.showSavingIcon && saving.state}
  <div
    class="absolute top-3 right-3 z-10 text-white p-2 rounded-sm bg-darkbg/90 border border-darkborderc shadow-lg pointer-events-none min-w-30"
  >
    <div class="flex items-center gap-2">
      <SaveIcon size={16} />
      <span class="text-xs font-semibold tabular-nums"
        >{Math.round(clampPercent(saving.percent))}%</span
      >
    </div>
    <div class="mt-1.5 h-1.5 w-full rounded-full bg-white/25 overflow-hidden">
      <div
        class="h-full bg-blue-400 transition-[width] duration-200"
        style:width={`${clampPercent(saving.percent)}%`}
      ></div>
    </div>
  </div>
{:else if $AccountWarning}
  <button class="absolute top-3 right-3 z-10 text-white bg-red-800 hover:bg-red-600 p-2 rounded-sm" onclick={() =>{
      alertMd($AccountWarning)
      $AccountWarning = ''
  }}>
      <OctagonAlert size={24} />
  </button>
{/if}
