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

  const formatEta = (etaSeconds: number) => {
    if (!Number.isFinite(etaSeconds) || etaSeconds < 0) {
      return "--";
    }
    const total = Math.round(Math.max(0, etaSeconds));
    if (total <= 0) {
      return "--";
    }
    if (total < 60) {
      return `~${total}s`;
    }
    const minutes = Math.floor(total / 60);
    const seconds = total % 60;
    if (seconds === 0) {
      return `~${minutes}m`;
    }
    return `~${minutes}m ${seconds}s`;
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
      <span class="text-[11px] text-white/85 tabular-nums"
        >{formatEta(saving.etaSeconds)}</span
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
