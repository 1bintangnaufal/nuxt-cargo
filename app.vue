<script setup>

// Table header
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

// Table row dummy data
const cargoData = ref([{
  id: 1,
  portOfCall: 'Shanghai, China',
  seaCraft: 'Seas the Day',
  voyageCode: 'SCSTD15',
  cargo: 'Corns',
  quantity: '10000 MT'
}, {
  id: 2,
  portOfCall: 'Singapore',
  seaCraft: 'On the Docks',
  voyageCode: 'SIOTD25',
  cargo: 'Soy Beans',
  quantity: '8000 MT'
}] || JSON.parse(localStorage.getItem('cargoData')));

// Options for select input
const cargoItems = ['Corns', 'Soy Beans', 'Rice', 'Wheat'];
const selectedCargoItems = ref(cargoItems[0])

// Initial form values for adding a new row (empty)
const state = reactive({
  portOfCall: '',
  seaCraft: '',
  voyageCode: '',
  cargo: '',
  quantity: 0
});

// Execute creating a new row
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

  // Set form back to empty after submit
  state.portOfCall = '';
  state.seaCraft = '';
  state.voyageCode = '';
  state.cargo = '';
  state.quantity = 0;
}

// Delete a row
const deleteCargoDatum = (id) => {
  const index = cargoData.value.findIndex(item => item.id === id);
  if (index !== -1) {
    cargoData.value.splice(index, 1);
    localStorage.setItem('cargoData', JSON.stringify(cargoData.value));
  }
}

// Modal trigger
const isModalOpen = ref(false);

// Initial state for updating a row, need ID
const updateState = reactive({
  id: null,
  portOfCall: '',
  seaCraft: '',
  voyageCode: '',
  cargo: '',
  quantity: 0
})

// Previous row data about to be updated
const previousData = (row) => {
  updateState.id = row.id;
  updateState.portOfCall = row.portOfCall;
  updateState.seaCraft = row.seaCraft;
  updateState.voyageCode = row.voyageCode;
  selectedCargoItems.value = row.cargo;
  updateState.quantity = parseFloat(row.quantity);
  isModalOpen.value = true;
}

// Execute updating row values
const updateCargoDatum = () => {
  const index = cargoData.value.findIndex(item => item.id === updateState.id);
  if (index !== -1) {
    updateState.cargo = selectedCargoItems.value;
    cargoData.value[index] = { ...updateState };
    localStorage.setItem('cargoData', JSON.stringify(cargoData.value));
    isModalOpen.value = false;
  }
}

// Dropdown actions
const menuItems = (row) => [
  [
    {
      label: 'Edit',
      icon: 'i-heroicons-pencil-square-20-solid',
      click: () => {
        isModalOpen.value = true;
        previousData(row);
      }
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

    <UModal v-model='isModalOpen'>
      <div class='p-4'>
        <UForm :state='updateState'>
          <UFormGroup label='Port of Call' name='portOfCall'>
            <UInput v-model='updateState.portOfCall' type='text' />
          </UFormGroup>

          <UFormGroup label='Sea Craft' name='seaCraft'>
            <UInput v-model='updateState.seaCraft' type='text' />
          </UFormGroup>

          <UFormGroup label='Voyage Code' name='voyageCode'>
            <UInput v-model='updateState.voyageCode' type='text' />
          </UFormGroup>

          <UFormGroup>
            <label>Cargo</label>
            <USelectMenu v-model='selectedCargoItems' :options='cargoItems' />
          </UFormGroup>

          <UFormGroup label='Quantity (MT)' name='quantity'>
            <UInput v-model='updateState.quantity' type='number' />
          </UFormGroup>

          <UButton @click='updateCargoDatum' class='mt-4'>Update</UButton>
        </UForm>
      </div>
    </UModal>
  </div>
</template>
