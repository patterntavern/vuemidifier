<template lang="pug">
div
  q-table.midiblocks-table(:data='midiblockValues' :columns='columns' row-key='uuid')
    template(v-slot:body-cell-actions='props')
      q-td(:props='props')
        q-btn.q-mr-lg(size='sm' color='secondary' @click='loadMidiblock(props)' icon='fas fa-folder-open') Load
        q-btn.q-mr-lg(size='sm' color='tertiary' @click='remixMidiblock(props)' icon='fas fa-copy') Remix
        q-btn(size='sm' color='negative' @click='deleteMidiblock(props)' icon='fas fa-trash') Delete
  DialogDeleteMidiblock(v-model='dialog.deleteMidiblock' :midiblock='dialogMidiblock')
</template>

<script>
import store from 'store'
import {mapState} from 'vuex'
import DialogDeleteMidiblock from '../dialog/DeleteMidiblock'
import {v4 as uuidv4} from 'uuid'

/**
 * Displays a table containing all available midiblocks
 */
export default {
  name: 'MidiblocksTable',

  components: {DialogDeleteMidiblock},

  computed: {
    ...mapState(['midiblocks']),

    midiblockValues () {
      return Object.values(this.midiblocks)
    }
  },

  data () {
    return {
      // The midiblock to use inside a dialog
      dialogMidiblock: null,
      
      // Dialog models
      dialog: {
        deleteMidiblock: false
      },
      
      // Table columns
      columns: [
        {
          label: 'Title',
          field: 'title',
          name: 'title',
          sortable: true,
          align: 'left'
        },
        {
          label: 'Description',
          field: 'description',
          name: 'description',
          sortable: true,
          align: 'left'
        },
        {
          label: 'Actions',
          name: 'actions',
          align: 'left'
        }
      ]
    }
  },

  methods: {
    /**
     * Loads the midiblock
     */
    loadMidiblock (props) {
      const midiblock = this.midiblocks[props.key]
      
      // Load block
      store.set('currentStudio', midiblock)
      this.$q.notify({
        type: 'positive',
        message: `Midilock "${midiblock.title}" loaded!`,
        timeout: 3000
      })

      // Reroute
      if (this.$route.name === 'Studio') {
        this.$store.commit('tally', 'reloads')
      } else {
        this.$router.push({name: 'Studio'})
      }
    },

    /**
     * Delete the midiblock
     */
    deleteMidiblock (props) {
      this.dialogMidiblock = this.midiblocks[props.key]
      this.dialog.deleteMidiblock = true
    },

    /**
     * Remix a midiblock
     */
    remixMidiblock (props) {
      const block = Object.assign({}, this.midiblocks[props.key])
      block.uuid = uuidv4()
      block.title += ' [Remixed]'

      this.$store.commit('set', [`midiblocks["${block.uuid}"]`, block])
      store.set('currentStudio', block)
      store.set('isStudioUnsaved', false)
      store.set('midiblocks', this.midiblocks)
      this.$q.notify({
        type: 'positive',
        message: `Midilock "${block.title}" remixed!`,
        timeout: 3000
      })

      // Reroute
      if (this.$route.name === 'Studio') {
        this.$store.commit('tally', 'reloads')
      } else {
        this.$router.push({name: 'Studio'})
      }
    }
  }
}
</script>

<style lang="sass">
.midiblocks-table tbody tr td:nth-child(1)
  font-size: 1.25em
  font-weight: bold
.midiblocks-table tbody tr td:nth-child(2)
  white-space: pre-wrap
</style>