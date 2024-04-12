<script setup lang="ts">
import { ref } from "vue";

import BaseBreadcrumb from "@/components/shared/BaseBreadcrumb.vue";
import UiTittleSubtitleCard from "@/components/shared/UiTittleSubtitleCard.vue";
import TheSalesOverview from "@/components/dashboards/analytical/TheSalesOverview.vue";
import TotalSales from "@/components/dashboards/analytical/TotalSales.vue";
import BlogCard from "@/components/dashboards/analytical/BlogCard.vue";
import NewsletterCampaign from "@/components/dashboards/analytical/NewsletterCampaign.vue";
import TheBandwidthUsage from "@/components/dashboards/analytical/TheBandwidthUsage.vue";
import TheDownloadCount from "@/components/dashboards/analytical/TheDownloadCount.vue";
import TheWeatherCard from '@/components/dashboards/analytical/TheWeatherCardVue.vue';
import TheProfileCard from '@/components/dashboards/analytical/TheProfileCard.vue';
import TheMyContacts from '@/components/dashboards/analytical/TheMyContacts.vue';
import TheActivityTimeline from '@/components/dashboards/analytical/TheActivityTimeline.vue'

const page = ref({ title: "Analytical" });
const breadcrumbs = ref([
  {
    text: "Dashboard",
    disabled: false,
    href: "#",
  },
  {
    text: "Analytical",
    disabled: true,
    href: "#",
  },
]);
</script>

<template>
  <v-row>
    <!---Sales overview--->
    <v-col cols="12" sm="12" lg="12">
      <TheSalesOverview :total="50" />
    </v-col>
    <!---Our visiters--->
  </v-row>
</template>
