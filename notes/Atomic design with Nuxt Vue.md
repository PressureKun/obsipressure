---
share: true
---
#vue #frontend #atomic-design #concept #methodology #nuxt

---
- [[#База|База]]
- [[#Основные концепции атомарного дизайна|Основные концепции атомарного дизайна]]
- [[#Шаблоны и страницы в Atomic Design - разница|Шаблоны и страницы в Atomic Design - разница]]
- [[#Пример структуры проекта в Nuxt.js|Пример структуры проекта в Nuxt.js]]
- [[#Примеры компонентов|Примеры компонентов]]
	- [[#Примеры компонентов#Атомы|Атомы]]
	- [[#Примеры компонентов#Молекулы|Молекулы]]
	- [[#Примеры компонентов#Организмы|Организмы]]
	- [[#Примеры компонентов#Шаблоны|Шаблоны]]
	- [[#Примеры компонентов#Страницы|Страницы]]
- [[#Пример структуры папок для нашего template_project|Пример структуры папок для нашего template_project]]
---

### База

Концепция атомарного дизайна (Atomic Design) была предложена Брэдом Фростом и представляет собой методологию для создания систем дизайна. Она разбивает интерфейсы на их мельчайшие частицы, которые затем можно комбинировать для создания более сложных компонентов и страниц. В контексте фреймворка Nuxt.js, который является надстройкой над Vue.js, атомарный дизайн может быть особенно полезен для создания масштабируемых и поддерживаемых приложений.

### Основные концепции атомарного дизайна

Атомарный дизайн состоит из пяти уровней:

1. **Атомы (Atoms)**: Это самые маленькие и неделимые компоненты, такие как кнопки, инпуты, лейблы и т.д.
2. **Молекулы (Molecules)**: Это комбинации атомов, которые вместе выполняют какую-то функцию. Например, форма поиска, состоящая из инпута и кнопки.
3. **Организмы (Organisms)**: Это более сложные компоненты, состоящие из молекул и атомов. Например, хедер сайта, включающий логотип, навигацию и форму поиска.
4. **Шаблоны (Templates)**: Это структуры страниц, состоящие из организмов, молекул и атомов, но без конкретного контента.
5. **Страницы (Pages)**: Это шаблоны, наполненные конкретным контентом.

---
### Шаблоны и страницы в Atomic Design - разница

1. **Шаблоны**:
   - **Назначение**: Шаблоны определяют общую компоновку и структуру страницы без конкретного содержимого. Они действуют как чертежи, которые указывают, где будут размещены различные компоненты (атомы, молекулы и организмы).
   - **Использование**: Шаблоны используются для обеспечения согласованности на разных страницах, предоставляя общую структуру. Они могут включать заполнители для динамического содержимого, которое будет заполняться страницами.
   - **Пример**: `MainTemplate.vue` может включать заголовок, подвал и основную область содержимого, куда будет вставляться конкретное содержимое страницы.

2. **Страницы**:
   - **Назначение**: Страницы являются конкретными экземплярами шаблонов, содержащими фактическое содержимое. Они используют шаблоны для поддержания согласованной компоновки, но заполняют заполнители уникальными данными и компонентами.
   - **Использование**: Страницы используются для отображения конечного содержимого, которое увидят пользователи. Каждая страница соответствует маршруту в вашем приложении.
   - **Пример**: `HomePage.vue` может использовать `MainTemplate.vue` и предоставлять конкретное содержимое, такое как приветственное сообщение, избранные статьи и т.д.

---

### Пример структуры проекта в Nuxt.js

Для реализации атомарного дизайна в Nuxt.js, можно организовать структуру проекта следующим образом:

```
nuxt-project/
├── assets/
│   └── ...
├── components/
│   ├── atoms/
│   │   └── Button.vue
│   ├── molecules/
│   │   └── FormInput.vue
│   ├── organisms/
│   │   └── Header.vue
│   └── templates/
│       └── ContentTemplate.vue
├── layouts/
│   └── default.vue
├── pages/
│   └── index.vue
│   └── about.vue
│   └── contact.vue
├── static/
│   └── ...
├── store/
│   └── ...
├── nuxt.config.js
└── package.json
```

---
### Примеры компонентов

#### Атомы

**Button.vue**
```vue
<template>
  <button :class="['btn', `btn--${type}`]" @click="onClick">
    <slot></slot>
  </button>
</template>

<script>
export default {
  props: {
    type: {
      type: String,
      default: 'primary'
    }
  },
  methods: {
    onClick() {
      this.$emit('click');
    }
  }
}
</script>

<style scoped>
.btn {
  padding: 10px 20px;
  border: none;
  cursor: pointer;
}
.btn--primary {
  background-color: blue;
  color: white;
}
.btn--secondary {
  background-color: gray;
  color: white;
}
</style>
```

**Input.vue**
```vue
<template>
  <input :type="type" :placeholder="placeholder" v-model="inputValue" @input="onInput" />
</template>

<script>
export default {
  props: {
    type: {
      type: String,
      default: 'text'
    },
    placeholder: {
      type: String,
      default: ''
    },
    value: {
      type: String,
      default: ''
    }
  },
  data() {
    return {
      inputValue: this.value
    }
  },
  methods: {
    onInput() {
      this.$emit('input', this.inputValue);
    }
  }
}
</script>

<style scoped>
input {
  padding: 10px;
  border: 1px solid #ccc;
  border-radius: 4px;
}
</style>
```

**Select.vue**
```vue
<template>
  <select v-model="selectedValue" @change="onChange">
    <option v-for="option in options" :key="option.value" :value="option.value">
      {{ option.text }}
    </option>
  </select>
</template>

<script>
export default {
  props: {
    options: {
      type: Array,
      required: true
    },
    value: {
      type: [String, Number],
      default: ''
    }
  },
  data() {
    return {
      selectedValue: this.value
    }
  },
  methods: {
    onChange() {
      this.$emit('change', this.selectedValue);
    }
  }
}
</script>

<style scoped>
select {
  padding: 10px;
  border: 1px solid #ccc;
  border-radius: 4px;
}
</style>
```

#### Молекулы

**SearchForm.vue**
```vue
<template>
  <form @submit.prevent="onSubmit">
    <Input v-model="searchQuery" placeholder="Search..." />
    <Button type="primary" @click="onSubmit">Search</Button>
  </form>
</template>

<script>
import Input from '@/components/atoms/Input.vue';
import Button from '@/components/atoms/Button.vue';

export default {
  components: {
    Input,
    Button
  },
  data() {
    return {
      searchQuery: ''
    }
  },
  methods: {
    onSubmit() {
      this.$emit('submit', this.searchQuery);
    }
  }
}
</script>

<style scoped>
form {
  display: flex;
  align-items: center;
}
</style>
```

#### Организмы

**Header.vue**
```vue
<template>
  <header>
    <div class="logo">MyLogo</div>
    <nav>
      <ul>
        <li><nuxt-link to="/">Home</nuxt-link></li>
        <li><nuxt-link to="/about">About</nuxt-link></li>
      </ul>
    </nav>
    <SearchForm @submit="onSearch" />
  </header>
</template>

<script>
import SearchForm from '@/components/molecules/SearchForm.vue';

export default {
  components: {
    SearchForm
  },
  methods: {
    onSearch(query) {
      console.log('Search query:', query);
    }
  }
}
</script>

<style scoped>
header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 20px;
  background-color: #f8f8f8;
}
.logo {
  font-size: 24px;
  font-weight: bold;
}
nav ul {
  list-style: none;
  display: flex;
  gap: 20px;
}
</style>
```





#### Шаблоны
```vue
<template>
  <div class="content-template">
    <slot></slot>
  </div>
</template>

<script>
export default {
  name: 'ContentTemplate'
}
</script>

<style scoped>
.content-template {
  background-color: #f9f9f9;
  padding: 20px;
  border-radius: 5px;
}
</style>
```

#### Страницы
```vue
<template>
  <ContentTemplate>
    <section>
      <h2>Welcome to the Home Page</h2>
      <p>This is an example of a home page using the Atomic Design methodology.</p>
    </section>
  </ContentTemplate>
</template>

<script>
import ContentTemplate from '~/components/templates/ContentTemplate.vue';

export default {
  name: 'HomePage',
  components: {
    ContentTemplate
  }
}
</script>

<style scoped>
section {
  padding: 20px;
  background-color: #fff;
  border-radius: 5px;
}
</style>
```

```vue
<template>
  <ContentTemplate>
    <section>
      <h2>About Us</h2>
      <p>This is an example of an about page using the Atomic Design methodology.</p>
    </section>
  </ContentTemplate>
</template>

<script>
import ContentTemplate from '~/components/templates/ContentTemplate.vue';

export default {
  name: 'AboutPage',
  components: {
    ContentTemplate
  }
}
</script>

<style scoped>
section {
  padding: 20px;
  background-color: #fff;
  border-radius: 5px;
}
</style>
```

```vue
<template>
  <ContentTemplate>
    <section>
      <h2>Contact Us</h2>
      <p>This is an example of a contact page using the Atomic Design methodology.</p>
      <FormInput label="Name" />
      <FormInput label="Email" />
      <Button>Submit</Button>
    </section>
  </ContentTemplate>
</template>

<script>
import ContentTemplate from '~/components/templates/ContentTemplate.vue';
import FormInput from '~/components/molecules/FormInput.vue';
import Button from '~/components/atoms/Button.vue';

export default {
  name: 'ContactPage',
  components: {
    ContentTemplate,
    FormInput,
    Button
  }
}
</script>

<style scoped>
section {
  padding: 20px;
  background-color: #fff;
  border-radius: 5px;
}
</style>
```

---

### Пример структуры папок для нашего template_project

```
template_project/
├── assets/
│   ├── js/
│   │   ├── utils/
│   │   │   ├── image-utils.js
│   │   │   ├── numbers-utils.js
│   │   │   └── date-time-utils.js
│   └── README.md
├── components/
│   ├── atoms/
│   │   ├── input/
│   │   │   ├── VInput.vue
│   │   │   ├── VInputThousands.vue
│   │   │   ├── VInputSelect.vue
│   │   │   └── VFile.vue
│   │   ├── button/
│   │   │   └── VButton.vue
│   │   ├── select/
│   │   │   ├── VSelect.vue
│   │   │   └── DropdownOption.vue
│   │   ├── tooltip/
│   │   │   └── VTooltip.vue
│   │   ├── slider/
│   │   │   ├── VSlider.vue
│   │   │   └── SliderDot.vue
│   │   ├── switch/
│   │   │   └── VSwitch.vue
│   │   ├── accordion/
│   │   │   └── VAccordionItem.vue
│   │   ├── popover/
│   │   │   └── VPopover.vue
│   │   ├── range/
│   │   │   └── VRange.vue
│   │   ├── skeleton/
│   │   │   └── VSkeleton.vue
│   │   ├── scrollbox/
│   │   │   └── VScrollBox.vue
│   │   └── и так далее...
│   ├── molecules/
│   │   ├── demos/
│   │   │   ├── VInputDemo.vue
│   │   │   ├── VInputSelectDemo.vue
│   │   │   └── VCalendarDemo.vue
│   │   ├── modals/
│   │   │   ├── MenuPanel.vue
│   │   │   ├── SlideMenu.vue
│   │   │   └── ModalFilterProjects.vue
│   │   ├── common/
│   │   │   ├── BottomSheet.vue
│   │   │   ├── ProgressBar.vue
│   │   │   └── LogoIdaOld.vue
│   │   ├── demo/
│   │   │   ├── ProjectsLoader.vue
│   │   │   ├── ProjectCard.vue
│   │   │   ├── ProjectsFilter.vue
│   │   │   └── ProjectsSelector.vue
│   │   └── и так далее...
│   ├── organisms/
│   │   ├── genplan/
│   │   │   ├── Visual.vue
│   │   │   ├── VisualObjects.vue
│   │   │   ├── VisualFlats.vue
│   │   │   └── VisualFilterModal.vue
│   │   ├── projects/
│   │   │   ├── ProjectsLoader.vue
│   │   │   ├── ProjectCard.vue
│   │   │   ├── ProjectsFilter.vue
│   │   │   └── ProjectsSelector.vue
│   │   └── и так далее...
│   ├── templates/ - по сути своей скелет-заготовки под страницы
│   │   ├── MainPageTemplate.vue
│   │   ├── UiPageTemplate.vue
│   │   ├── UiPageTemplate.vue
│   │   └── Другие темплейт-компоненты для страниц
├── layouts/
│   ├── default.vue
│   └── custom.vue
├── middleware/
│   ├── uikit-redirect.js
├── mixins/
│   ├── uiContainerObserver.js
├── pages/ - а вот тут уже конкретные реализации templates
│   ├── index.vue
│   ├── about.vue
│   ├── dashboard.vue
│   ├── ui-kit/
│   │   ├── base.vue
│   │   ├── cards.vue
│   │   ├── filters.vue
│   │   └── inputs.vue
│   ├── examples/
│   │   ├── breakpointsTest.vue
│   │   ├── genplan.vue
│   │   ├── imgProxy.vue
│   │   ├── projectsFilter.vue
│   │   └── tabledData.vue
│   ├── iframe/
│   │   └── index.vue
│   └── release.vue
├── plugins/
├── static/
├── store/
├── nuxt.config.js
└── package.json
```


### Источники
- https://dev.to/weezer924/using-atomic-design-with-nuxt-js-and-have-a-great-hacking-time-3lc5
- https://medium.com/@kevinkurniawan97/atomic-design-with-vue-fa1b50a1251e
- https://dev.to/berryjam/introducing-atomic-design-in-vuejs-1l2h
- https://shahbaziamir.medium.com/atomic-design-in-nuxtjs-and-tailwind-css-1b391e845f33