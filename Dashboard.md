
```dataviewjs
const fecha = new Date().toLocaleDateString("es-ES", {
	weekday: "long",
	day: "numeric",
	month: "long",
	year: "numeric"
});
dv.el("H1",fecha.charAt(0).toUpperCase() + fecha.slice(1), { cls: "home-fecha" });
```
## Tareas pendientes

```dataviewjs
// Todas las tareas pendientes del vault
let tareas = dv.pages('""')
    .file.tasks
    .where(t => !t.completed);

for (let tarea of tareas) {
    const pagina = dv.page(tarea.path);
    if (pagina && pagina.type === "project") {
        const nombre = pagina.title ?? pagina.file.name;
        tarea.visual = `\`📁 ${nombre}\`: ${tarea.text}`;
    } else if (pagina && pagina.type === "121") {
        const nombre = pagina.title ?? pagina.file.name;
        tarea.visual = `\`👤 ${nombre}\`: ${tarea.text}`;
    }
}

dv.taskList(tareas, false);
```
---
```dataviewjs
const pages = dv.pages()
    .where(p => p.file.tasks?.length);

for (const page of pages) {
    for (const task of page.file.tasks) {

        // Tags de la tarea
        const taskTags = task.tags ?? [];

        // Tags de la página
        const pageTags = page.file.tags ?? [];

        // Tags efectivos de la tarea
        const effectiveTags = [
            ...new Set([
                ...taskTags,
                ...pageTags
            ])
        ];

        // Aquí puedes usar effectiveTags
        // como si fueran los tags propios de la tarea
    }
}
```
## 📄 Notas modificadas recientemente

 ```dataview
 TABLE file.mtime AS "Última edición"
 FROM ""
 SORT file.mtime DESC
 LIMIT 5
 ```

---

## 📅 Últimas reuniones

```dataview
TABLE date AS "Fecha", meeting_type AS "Tipo", project AS "Proyecto", summary AS "Resumen"
FROM "Meetings"
WHERE type = "meeting"
SORT date DESC
LIMIT 10
```

---

## 📁 Proyectos activos

```dataview
TABLE status AS "Status", description AS "Description"
FROM "01.Projects"
WHERE type = "project" AND status != "done"
SORT file.mtime DESC
```
