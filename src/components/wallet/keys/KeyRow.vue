<template>
  <div
    class="addressItem"
    :selected="is_default"
  >
    <div class="rows">
      <div class="detail">
        <div>
          <p class="addressVal"><span>{{ $t('keys.address') }}</span>{{ address }}</p>
        </div>
      </div>

      <div>
        <p v-if="Object.keys(balances).length === 0" class="balance_empty">{{ $t('keys.empty') }}</p>
        <div v-else class="addressBalance bal_cols">
          <p>This address has: </p>
          <div class="bal_rows">
            <p v-for="bal in balances" :key="bal.id">
              {{ bal.toString() }} <b>{{ bal.symbol }}</b>
            </p>
          </div>

        </div>
      </div>
    </div>
    <div class="buts">
      <button v-if="!is_default" class="selBut" @click="select">
        <span>Make Default</span>
      </button>

      <button v-if="!is_default" @click="remove"><fa icon="trash" /> Remove Key</button>

    </div>
  </div>
</template>
<script>

export default {
  props: {
    address: {
      type: String,
      required: true
    },
    // eslint-disable-next-line vue/prop-name-casing
    is_default: {
      type: Boolean,
      default: false
    }
  },
  computed: {
    balance() {
      return this.$store.state.Assets.assetsDict
    },
    // balances() {
    //   const res = {}

    //   const utxos = this.$store.getters['Assets/addressUTXOs']
    //   const addr = this.address
    //   const addrStrip = addr.split('-')[1]

    //   const addrUtxos = utxos[addrStrip]
    //   if (addrUtxos) {
    //     for (var n = 0; n < addrUtxos.length; n++) {
    //       const utxo = addrUtxos[n]
    //       const utxoOut = utxo.getOutput()

    //       // console.log(utxo);
    //       const amount = utxoOut.getAmount()
    //       const assetIdBuff = utxo.getAssetID()
    //       const assetId = bintools.avaSerialize(assetIdBuff)

    //       const assetObj = this.balance[assetId]
    //       const asset = res[assetId]
    //       if (!asset) {
    //         const name = assetObj.name
    //         const symbol = assetObj.symbol
    //         const denomination = assetObj.denomination

    //         const newAsset = new AvaAsset(assetId, name, symbol, denomination)
    //         newAsset.addBalance(amount)

    //         res[assetId] = newAsset
    //       } else {
    //         asset.addBalance(amount)
    //       }
    //     }
    //   }
    //   return res
    // },
    addrRaw() {
      return this.address.split('-')[1]
    },
    keyPair() {
      return keyChain.getKey(bintools.parseAddress(this.address, 'X'))
    },
    publicKey() {
      return this.keyPair.getPublicKeyString()
    }
  },
  mounted() {
    // console.log(this.$store.state);
  },
  methods: {
    remove() {
      this.$emit('remove', this.address)
    },
    select() {
      this.$emit('select', this.address)
    }
  }
}
</script>
<style scoped lang="scss">
    .addressItem{
        font-size: 12px;
        /*display: flex;*/
        /*align-items: center;*/
        display: grid;
        grid-template-columns: 1fr max-content;
        grid-gap: 15px;
        /*background-color: #F5F6FA;*/
        overflow: auto;

        > *{
            align-self: center;
            overflow: auto;
        }
    }

    .buts{
        display: flex;
        flex-direction: row;
    }

    .rows{
        overflow: auto;
    }
    .addressItem .selBut{
        /*flex-basis: 14px;*/
        /*height: 14px;*/
        /*width: 14px;*/
        /*border-radius: 14px;*/
        color: #ccc;
        flex-shrink: 0;

        span{
            font-size: 12px;
            line-height: normal;
        }
    }

    .addressItem{
        &[selected]{
            .selBut{
                /*background-color: transparent;*/
            }
        }
        .selBut{
            flex-grow: 1;
            background-color: #C0C0CD;
            color: #fff;
            padding: 4px 8px;
            margin-right: 15px;
        }
    }

    .detail{
        /*margin-left: 20px;*/
        /*flex-grow: 1;*/
        overflow: auto;
        display: grid;
        grid-template-columns: 1fr 1fr;
        column-gap: 15px;
    }

    .label{
        /*font-size: 13px;*/
        font-weight: bold;
    }
    .addressVal{
        /*word-break: break-all;*/
        overflow: auto;
        text-overflow: ellipsis;
        white-space: nowrap;

        span{
            font-weight: normal;
            margin-right: 8px;
        }
    }

    .del{
        align-self: start;
        opacity: 0.4;

        &:hover{
            opacity: 1;
        }
    }

    .addressBalance{
        display: flex;
        white-space: nowrap;
        color: #2960CD;
        .bal_rows p{
            font-weight: bold;
            /*background-color: #ebedf5;*/
            padding: 0px 8px;
            margin-bottom: 4px;
        }
        p{

            /*border: 1px solid #ebedf5;*/
            border-radius: 3px;
        }
    }

    .bal_cols{
        display: flex;
    }

    .bal_rows{
        display: flex;
        flex-direction: column;
    }

    .balance_empty{
        color: #2960CD;
    }
    /*.addressItem[selected]{*/
    /*    .addressBalance{*/
    /*        p{*/
    /*            background-color: #b1c9fb;*/
    /*        }*/
    /*    }*/
    /*}*/

</style>
