<template lang="pug">
  gas-meter-device-panel(
    v-bind="$attrs"
    :meter-reading="consumption ? consumption.value / 1000 : undefined"
  )
    template(#actions)
      slot(name="actions")
    template(#properties)
      slot(name="properties")
    template(#overlay)
      slot(name="overlay")
</template>

<script lang="ts">
import { DevicePanelQuery } from '@/features/core/components/device-panel/__generated__/DevicePanelQuery';
import MetricMapMixin, { DataPoint } from '@/features/core/components/device-panel-adapters/metric-map-mixin';
import DeviceRoleMapMixin from '@/features/core/components/mixins/device-role-map';
import { ObjectProp } from '@/util/prop-decorators';
import { Component, Mixins } from 'vue-property-decorator';
import GasMeterDevicePanel from './GasMeterDevicePanel.vue';

@Component({ components: { GasMeterDevicePanel } })
export default class GasMeterDevicePanelAdapter extends Mixins(DeviceRoleMapMixin, MetricMapMixin) {
  @ObjectProp(true)
  private readonly device!: DevicePanelQuery['data'];

  private get consumption(): DataPoint | undefined {
    return this.latestDataPoint('Consumption');
  }
}
</script>
