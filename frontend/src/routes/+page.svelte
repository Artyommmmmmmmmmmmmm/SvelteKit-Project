<script>
  import { createForm } from "felte";
  import { z } from "zod";
  import axios from "axios";

  // Определение схемы валидации
  const schema = z.object({
    title: z.string().min(1, "Title is required"),
    description: z.string().optional(),
    completed: z.boolean(),
  });

  const { form, errors } = createForm({
    onSubmit: async (values) => {
      try {
        await axios.post("http://127.0.0.1:8000/api/tasks/", values);
        alert("Task created successfully!");
      } catch (err) {
        console.error(err);
        alert("Failed to create task.");
      }
    },
    validate: schema.parse,
  });
</script>

<form use:form>
  <label>
    Title:
    <input type="text" name="title" />
  </label>
  {#if errors.title}
    <p>{errors.title}</p>
  {/if}

  <label>
    Description:
    <textarea name="description"></textarea>
  </label>

  <label>
    Completed:
    <input type="checkbox" name="completed" />
  </label>

  <button type="submit">Create Task</button>
</form>