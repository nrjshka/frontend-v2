<script lang="ts" setup>
import {
  isLiquidityBootstrapping,
  isBoosted,
} from '@/composables/usePoolHelpers';
import { poolMetadata } from '@/lib/config/metadata';
import { Pool } from '@/services/pool/types';
import { PoolMetadata } from '@/types/pools';
import BalChipNew from '@/components/chips/BalChipNew.vue';
import BoostedChip from '@/components/chips/BoostedChip.vue';
import PoolWarningTooltip from '@/components/pool/PoolWarningTooltip.vue';

type Props = {
  pool: Pool;
};

defineProps<Props>();
</script>

<template>
  <div class="flex items-center">
    <BalTooltip v-if="isBoosted(pool)" :text="$t('boostedTooltip')" width="56">
      <template #activator>
        <BoostedChip
          :metadata="poolMetadata(pool.id) as PoolMetadata"
          class="ml-1"
        />
      </template>
    </BalTooltip>

    <BalChip
      v-if="isLiquidityBootstrapping(pool.poolType)"
      label="LBP"
      color="amber"
      class="text-xs font-medium"
    />
    <BalChipNew v-else-if="pool?.isNew" class="text-lg font-medium" />

    <PoolWarningTooltip :pool="pool" />
  </div>
</template>
