<script setup>

const columns = [{
  key: 'portOfCall',
  label: 'Port Of Call'
}, {
  key: 'seaCraft',
  label: 'Sea Craft'
}, {
  key: 'voyageCode',
  label: 'Voyage Code'
}, {
  key: 'cargo',
  label: 'Cargo'
}, {
  key: 'quantity',
  label: 'Quantity'
}, {
  key: 'actions'
}];

const cargoData = ref([{
  id: 1,
  portOfCall: 'Shanghai, China',
  seaCraft: 'Seas the Day',
  voyageCode: 'SCSTD15',
  cargo: 'Corns',
  quantity: '10,000 MT'
}, {
  id: 2,
  portOfCall: 'Singapore',
  seaCraft: 'On the Docks',
  voyageCode: 'SIOTD25',
  cargo: 'Soy Beans',
  quantity: '8,000 MT'
}] || JSON.parse(localStorage.getItem('cargoData')));

const cargoItems = ['Corns', 'Soy Beans', 'Rice', 'Wheat'];
const selectedCargoItems = ref(cargoItems[0])

const state = reactive({
  portOfCall: '',
  seaCraft: '',
  voyageCode: '',
  cargo: '',
  quantity: ''
});

const insertCargoDatum = () => {
  const newRow = {
    id: Date.now(),
    portOfCall: state.portOfCall,
    seaCraft: state.seaCraft,
    voyageCode: state.voyageCode,
    cargo: selectedCargoItems.value,
    quantity: state.quantity + ' MT'
  }

  cargoData.value.push(newRow)
  localStorage.setItem('cargoData', JSON.stringify(cargoData.value));

  state.portOfCall = '';
  state.seaCraft = '';
  state.voyageCode = '';
  state.cargo = '';
  state.quantity = '';
}

const deleteCargoDatum = (id) => {
  const index = cargoData.value.findIndex(item => item.id === id);
  if (index !== -1) {
    cargoData.value.splice(index, 1);
    localStorage.setItem('cargoData', JSON.stringify(cargoData.value));
  }
}

const menuItems = (row) => [
  [
    {
      label: 'Edit',
      icon: 'i-heroicons-pencil-square-20-solid',
    },
    {
      label: 'Delete',
      icon: 'i-heroicons-trash-20-solid',
      click: () => deleteCargoDatum(row.id)
    }
  ]
];

</script>



<template>
  <!-- <NuxtWelcome /> -->
  <div class='p-36 flex flex-col gap-10'>
    <UContainer class='w-full'>
      <UCard>
        <div class='flex flex-col justify-center align-middle gap-10'>
          <h1 class='text-xl'>
            Insert a new row
          </h1>

          <UForm :state='state'>
            <div class='grid grid-cols-2 gap-5'>
              <UFormGroup label='Port of Call' name='portOfCall'>
                <UInput v-model='state.portOfCall' type='text' />
              </UFormGroup>

              <UFormGroup label='Sea Craft' name='seaCraft'>
                <UInput v-model='state.seaCraft' type='text' />
              </UFormGroup>

              <UFormGroup label='Voyage Code' name='voyageCode'>
                <UInput v-model='state.voyageCode' type='text' />
              </UFormGroup>

              <UFormGroup>
                <label>Cargo</label>
                <USelectMenu v-model='selectedCargoItems' :options='cargoItems' />
              </UFormGroup>

              <UFormGroup label='Quantity (MT)' name='quantity'>
                <UInput v-model='state.quantity' type='number' />
              </UFormGroup>
            </div>
          </UForm>

          <UButton type='submit' @click='insertCargoDatum' class='justify-center' variant='soft'>
            Insert
          </UButton>
        </div>
      </UCard>
    </UContainer>

    <UContainer class='w-full'>
      <UCard>
        <UTable :columns='columns' :rows='cargoData'>
          <template #actions-data='{ row }'>
            <UDropdown :items='menuItems(row)'>
              <UButton color='gray' variant='ghost' icon='i-heroicons-ellipsis-horizontal-20-solid' />
            </UDropdown>
          </template>
        </UTable>
      </UCard>
    </UContainer>
  </div>
</template>
