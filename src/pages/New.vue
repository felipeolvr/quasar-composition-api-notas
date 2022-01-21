<script>
import Container from "src/components/Container.vue";
import { defineComponent, reactive } from "vue";
import { useLocalNotes } from "src/helper";
import { useRouter } from "vue-router";

export default defineComponent({
  components: { Container },
  name: "PageNew",
  setup() {
    const router = useRouter();
    const notes = useLocalNotes();

    const note = reactive({
      title: "",
      description: "",
      content: "",
    });

    const submit = () => {
      notes.value.unshift({
        ...note,
        createdAt: Date.now(),
        updatedAt: Date.now(),
      });

      router.push("/");

      note.title = "";
      note.description = "";
      note.content = "";
    };
    return { note, submit };
  },
});
</script>

<template>
  <q-page padding>
    <Container>
      <h3>Nova Nota</h3>
      <form @submit="submit">
        <q-input class="q-mt-sm" outlined v-model="note.title" label="Título" />

        <q-input
          class="q-mt-sm"
          outlined
          v-model="note.description"
          label="Descrição"
          dense
        />

        <q-card flat bordered class="q-mt-sm">
          <q-editor v-model="note.content" min-height="5rem" />
        </q-card>

        <div class="q-mt-md">
          <q-btn color="grey" to="/" type="reset">Cancelar</q-btn>
          <q-btn class="q-ml-sm" color="positive" type="submit"> Criar </q-btn>
        </div>
      </form>
    </Container>
  </q-page>
</template>
