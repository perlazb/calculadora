<script setup lang="ts">
import TheWelcome from '../components/TheWelcome.vue'
</script>

export default defineComponent({ components: { buttonVue }, setup(){ return{ numero: ref(0) } }.
methods: { sumNum(){ this.numero = this.numero + 1 } } })
