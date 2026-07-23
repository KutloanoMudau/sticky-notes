# Sticky Notes Application – Design Diagrams

These diagrams support the Django **sticky_notes** project. You can view them in
any Mermaid-compatible viewer (e.g. [mermaid.live](https://mermaid.live)) or
export them as PNG/PDF for submission.

---

## 1. Use Case Diagram

```mermaid
flowchart LR
    User((User))

    subgraph StickyNotes["Sticky Notes Application"]
        UC1[View all notes]
        UC2[Create a note]
        UC3[Read a note]
        UC4[Update a note]
        UC5[Delete a note]
    end

    User --> UC1
    User --> UC2
    User --> UC3
    User --> UC4
    User --> UC5
```

**Description:** A user interacts with the application to manage personal sticky
notes. All five CRUD operations are available from the web interface.

---

## 2. Sequence Diagram – Create a Note

```mermaid
sequenceDiagram
    actor User
    participant Browser
    participant View as NoteCreateView
    participant Form as NoteForm
    participant Model as Note Model
    participant DB as Database

    User->>Browser: Open "New Note" page
    Browser->>View: GET /notes/create/
    View->>Form: Display empty form
    Form-->>Browser: Render note_form.html
    User->>Browser: Enter title and content, submit
    Browser->>View: POST /notes/create/
    View->>Form: Validate submitted data
    Form-->>View: Valid
    View->>Model: save()
    Model->>DB: INSERT note row
    DB-->>Model: Success
    Model-->>View: Note instance
    View-->>Browser: Redirect to note list
    Browser-->>User: Show updated notes
```

---

## 3. Class Diagram

```mermaid
classDiagram
    class Note {
        +int id
        +str title
        +str content
        +datetime created_at
        +datetime updated_at
        +__str__()
    }

    class NoteForm {
        +title: CharField
        +content: TextField
        +clean()
    }

    class NoteListView {
        +get_queryset()
    }

    class NoteDetailView {
        +get_object()
    }

    class NoteCreateView {
        +form_valid()
    }

    class NoteUpdateView {
        +form_valid()
    }

    class NoteDeleteView {
        +delete()
    }

    NoteForm ..> Note : creates/updates
    NoteListView ..> Note : reads many
    NoteDetailView ..> Note : reads one
    NoteCreateView ..> NoteForm : uses
    NoteUpdateView ..> NoteForm : uses
    NoteDeleteView ..> Note : deletes
```

---

## Design principles applied

| Principle | How it is used |
|-----------|----------------|
| **Separation of concerns** | Models (data), views (logic), templates (presentation), forms (validation) |
| **DRY** | `base.html` extended by all page templates |
| **Layered architecture** | Presentation → Business (views) → Persistence (models/DB) |
| **CRUD** | Full create, read, update, delete for notes |
