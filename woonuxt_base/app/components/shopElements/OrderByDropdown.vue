<script setup>
const { getOrderQuery, setOrderQuery } = await useSorting();
const { storeSettings } = useAppConfig();
const route = useRoute();

// Reactive refs که се синхронизират с URL-а
const orderby = ref('date');
const order = ref('DESC');

// Функция за синхронизация със състоянието от URL
const syncWithURL = () => {
  const currentOrder = getOrderQuery();
  orderby.value = currentOrder.orderBy || 'date';
  order.value = currentOrder.order || 'DESC';
};

// Инициална синхронизация
syncWithURL();

// Флаг за предотвратяване на endless loops
let isUpdatingFromURL = false;

// Слушаме за промени в URL за да синхронизираме състоянието
watch(
  () => route.query,
  () => {
    if (!isUpdatingFromURL) {
      isUpdatingFromURL = true;
      syncWithURL();
      nextTick(() => {
        isUpdatingFromURL = false;
      });
    }
  },
  { deep: true },
);

// Update the URL when the dropdown is changed
watch(
  [orderby, order],
  () => {
    if (!isUpdatingFromURL) {
      setOrderQuery(orderby.value, order.value);
    }
  },
  { flush: 'post' },
);
</script>

<template>
  <div class="inline-flex ml-auto -space-x-px shadow-sm rounded-m isolate">
    <button
      class="relative inline-flex items-center p-2 text-sm font-medium text-gray-500 bg-white border border-gray-300 rounded-l-md hover:bg-gray-50 focus:z-20"
      aria-label="Sort"
      @click="order = order === 'ASC' ? 'DESC' : 'ASC'">
      <Icon name="ion:filter-outline" size="18" :class="order === 'ASC' ? 'rotate-180' : ''" class="transition-transform transform transform-origin-center" />
    </button>
    <select id="orderby-dropdown" v-model="orderby" class="bg-white rounded-l-none" aria-label="Order by">
      <option value="date">{{ $t('messages.general.latest') }}</option>
      <option value="alphabetically">{{ $t('messages.general.alphabetically') }}</option>
      <option value="price">{{ $t('messages.shop.price') }}</option>
      <option v-if="storeSettings.showReviews" value="rating">{{ $t('messages.shop.rating') }}</option>
      <option value="discount">{{ $t('messages.shop.discount') }}</option>
    </select>
  </div>
</template>
