<template>
    <div class="payments">
        <h2>Payments by sections</h2>
        <div class="it">
          <p>IT -</p>
          <span v-if="ITPayments">{{ITPayments.toFixed(2)}} PLN</span>
        </div>
        <div class="administration">
          <p>Administration -</p>
          <span v-if="administrationPayments">{{administrationPayments.toFixed(2)}} PLN</span>
        </div>
        <div class="merchant">
          <p>Merchant -</p>
          <span v-if="merchantPayments">{{merchantPayments.toFixed(2)}} PLN</span>
        </div>

        <h2>Total payments</h2>
        <div class="all-sections">
          <p>Total -</p>
          <span v-if="merchantPayments">{{(merchantPayments + administrationPayments + ITPayments).toFixed(2)}} PLN</span>
        </div>
      </div>
</template>

<script>
export default {
    name: 'TotalPayments',
    props: {
        ITPayments: Number,
        administrationPayments: Number,
        merchantPayments: Number,
    },
}
</script>

<style scoped>
.payments{
  margin: 0 10rem;
  text-align: center;
}
.it,
.administration,
.merchant,
.all-sections{
  display: flex;
  justify-content: center;
}
span,
p{
  font-size: 2.2rem;
  margin: .5rem 0;
}
p{padding-right: .3rem;}
</style>