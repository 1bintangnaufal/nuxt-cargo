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

const state = reactive({
  portOfCall: '',
  seaCraft: '',
  voyageCode: '',
  cargo: '',
  quantity: ''
});

const insertCargoDatum = () => {
  const newRow = {
    id: cargoData.length + 1,
    portOfCall: state.portOfCall,
    seaCraft: state.seaCraft,
    voyageCode: state.voyageCode,
    cargo: state.cargo,
    quantity: state.quantity
  }

  cargoData.value.push(newRow)
  localStorage.setItem('cargoData', JSON.stringify(cargoData.value));

  state.portOfCall = '';
  state.seaCraft = '';
  state.voyageCode = '';
  state.cargo = '';
  state.quantity = '';

}

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

              <UFormGroup label='Cargo' name='cargo'>
                <UInput v-model='state.cargo' type='text' />
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
        <UTable :columns='columns' :rows='cargoData' />
      </UCard>
    </UContainer>
  </div>
</template>
