---
theme: default
background: https://cover.sli.dev
title: Code Sharing
class: text-center
# https://sli.dev/features/drawing
drawings:
  persist: false
# slide transition: https://sli.dev/guide/animations.html#slide-transitions
transition: slide-left
# enable MDC Syntax: https://sli.dev/features/mdc
mdc: true
# take snapshot for each slide in the overview
overviewSnapshots: true
---

### Code Sharing
# 2024 Oct 17

---

# Sharing Topics

- ChatBotMessageBlock Typing
- Draggable
- Collapsible

- Validatable
- ~~Vue3.5~~

---

## ChatBotMessageBlock Typing
[reference](https://www.notion.so/Chatbot-Message-Blocks-3c395d93d9d0485794273a8450df0774#0aff4ee3c7bf48df9f2961e3c7973a9b)

could be checked at `src/types/ChatbotMessageBlock.ts`
```ts {*}{maxHeight:'37vh'}
export interface T_BasicMessageBlock {
  id: string
  message?: string
}

export type T_InvoiceMessageBlock = T_BasicMessageBlock & {
  type: 31
  extension: {
    invoiceCampaignId: string
  }
};

export type T_ImageMessageBlock = T_BasicMessageBlock & {
  type: 303
  photoUrl: string
};

export type T_ImageMapAction = {
  tags: string[]
} & (
  | {
    type: 'uri'
    uri: string
  }
  | {
    type: 'video'
    videoUrl: string
    externalLink: string
    ctaLabel: string
  }
  | {
    type: 'message'
    text: string
  }
  | {
    type: 'postback'
    chatbotId: string
    blockId: string
  }
);

export type T_ImageMapMessageBlock = T_BasicMessageBlock & {
  type: 305
  extension: {
    imagemapType: 0 | 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8
    actions: T_ImageMapAction[]
    altText: string
    baseUrl: string
    ratio?: string // only appear in imagemapType 0
  }
};

export type T_VideoMessageBlock = T_BasicMessageBlock & {
  type: 307
  videoUrl: string
  videoFilename: string
  videoPreviewImgUrl: string
};

export type T_CouponMessageBlock = T_BasicMessageBlock & {
  type: 310
  extension: {
    couponId: string
  }
};

export type T_ImageCarouselAction =
  | {
    type: 'postback'
    uuid: string
    tags: string[]
    saveAttr: {
      key: string
      value: boolean
      type: 5
    }
  }
  | {
    type: 'url'
    url: string
    tags: string[]
  }
  | {
    type: 'system'
    subtype: string // required when type is system
    tags: string[]
  };

export interface T_ImageCarousel {
  imageUrl: string
  overlapImage?: {
    imageUrl: string
    width: number
    height: number
    offsetTop: number
    offsetStart: number
    cornerRadius: number
  }
  action?: T_ImageCarouselAction[] // 點擊圖片後的動作
  buttons: {
    label: string
    style: string
    color: string
    action: T_ImageCarouselAction
  }[]
}

export type T_ImageCarouselMessageBlock = T_BasicMessageBlock & {
  type: 311
  extension: {
    bgImgWidth: number
    bgImgHeight: number
    imageAspectRatio: string
    creationDate: string
    modificationDate: string
    status: number
    carousel: T_ImageCarousel[]
  }
};

export type T_WhatsappStickerPackMessageBlock = T_BasicMessageBlock & {
  type: 312
  extension: {
    stickerPackId: string
  }
};

export type T_ImageMapV2MessageBlock = T_BasicMessageBlock & {
  type: 313
  message: string
  extension: {
    imagemapId: string
  }
};

export type T_WhatsappFlowMessageBlock = T_BasicMessageBlock & {
  type: 314
  message: string
  extension: {
    flowId: string
    screen: string
    header: {
      type: string
      image: string
      text: string
    }
    body: string
    footer: string
    clickToAction: string
  }
};

export type T_QuietReplyMessageBlock = T_BasicMessageBlock & {
  type: 401
  message: string
  extension: {
    title: string
    replies: {
      button_id: string
      type: string
      title: string
      description: string
      uuid: string | null
      tags: string[]
      saveAttr?: {
        key: string
        value: string
        type: number
      }
    }[]
  }
};

export type T_UserInputMessageBlock = T_BasicMessageBlock & {
  type: 501
  message: string
  extension: {
    nextblockUUID: string
    saveAttr: string
  }
};

export type T_JSONApiMessageBlock = T_BasicMessageBlock & {
  type: 502
  message: string
  extension: {
    apiEndpoint: string
    headers: {
      key: string
      value: string
    }[]
    requestBody: string
    successReplyBlock: string
    failedReplyBlock: string
  }
};

export type T_DelayedReplyMessageBlock = T_BasicMessageBlock & {
  type: 503
  message: string
  extension: {
    delayTime: number
    cancelIfUserInteract: boolean
    replyBlock: string
  }
};

export type T_ConditionFilterSystemAttributeKeys =
  | 'system:member_id'
  | 'system:phone'
  | 'system:email'
  | 'system:tel_verified'
  | 'system:nineone_app_bind'
  | 'system:pointBalance'
  | 'system:accumulatedPoints'
  | 'system:loyaltyPoint:balance'
  | 'system:loyaltyPoint:accumulation';

export enum E_AttributeTypes {
  TEXT = 1,
  NUMBER = 2,
  DATE = 3,
  DATETIME = 4,
  BOOLEAN = 5
}

export type T_DefaultTextAttribute =
  | 'equals'
  | 'notEquals'
  | 'contains'
  | 'notContains'
  | 'exists'
  | 'doesNotExist';
export type T_DefaultNumberAttribute =
  | 'greaterThan'
  | 'greaterThanOrEqualTo'
  | 'lessThan'
  | 'lessThanOrEqualTo'
  | 'equals'
  | 'notEquals'
  | 'between'
  | 'notBetween'
  | 'exists'
  | 'doesNotExist';
export type T_DefaultDateAttribute =
  | 'withinPastCoupleDays'
  | 'notWithinPastCoupleDays'
  | 'beforePastCoupleDays'
  | 'withinFutureCoupleDays'
  | 'afterFutureCoupleDays'
  | 'notBetween'
  | 'equals'
  | 'exists'
  | 'doesNotExist';
export type T_DefaultDatetimeAttribute =
  | 'withinPastCoupleMilliseconds'
  | 'notWithinPastCoupleDays'
  | 'beforePastCoupleMilliseconds'
  | 'withinFutureCoupleMilliseconds'
  | 'afterFutureCoupleMilliseconds'
  | 'between'
  | 'notBetween'
  | 'exists'
  | 'doesNotExist';
export type T_DefaultBooleanAttribute = 'equals' | 'exists' | 'doesNotExist';

export interface T_OperatorByType {
  tags: 'matchOne' | 'matchAll'
  customAttribute: {
    TEXT: T_DefaultTextAttribute
    NUMBER: T_DefaultNumberAttribute
    DATE: T_DefaultDateAttribute
    DATETIME: T_DefaultDatetimeAttribute
    BOOLEAN: T_DefaultBooleanAttribute
  }
  systemAttribute: {
    TEXT: 'exists' | 'doesNotExist'
    NUMBER:
      | 'greaterThan'
      | 'greaterThanOrEqualTo'
      | 'lessThan'
      | 'lessThanOrEqualTo'
      | 'equals'
      | 'notEquals'
      | 'between'
      | 'notBetween'
    DATE: T_DefaultDateAttribute
    DATETIME: T_DefaultDatetimeAttribute
    BOOLEAN: 'equals'
  }
}

export interface T_BasicConditionFilter {
  filterType: 'tag' | 'customAttribute' | 'systemAttribute'
  values: string[] | number[] | boolean[]
}

export type T_TagConditionFilter = T_BasicConditionFilter & {
  filterType: 'tag'
  values: string[]
  attributeKey?: string
  operator: any
  type?: number
  attributeType?: number
};

export type T_CustomAttributeConditionFilter = T_BasicConditionFilter & {
  filterType: 'customAttribute'
  attribute: string
  attributeType: number
  // value of E_AttributeTypes
  type: (typeof E_AttributeTypes)[keyof typeof E_AttributeTypes]
  // value of T_OperatorByType['customAttribute']
  operator: T_OperatorByType['customAttribute'][keyof T_OperatorByType['customAttribute']]
  attributeKey?: string
};

export type T_SystemAttributeConditionFilter = T_BasicConditionFilter & {
  filterType: 'systemAttribute'
  attribute: T_ConditionFilterSystemAttributeKeys
  // value of E_AttributeTypes
  type: (typeof E_AttributeTypes)[keyof typeof E_AttributeTypes]
  // value of T_OperatorByType['systemAttribute']
  operator: T_OperatorByType['systemAttribute'][keyof T_OperatorByType['systemAttribute']]
  attributeKey?: string
  attributeType?: number
};

export type T_ConditionMessageBlock = T_BasicMessageBlock & {
  type: 504
  message: string
  extension: {
    operator: 'and' | 'or'
    filters: (
      | T_TagConditionFilter
      | T_CustomAttributeConditionFilter
      | T_SystemAttributeConditionFilter
    )[]
    matchedReplyBlock?: string
    notMatchedReplyBlock?: string
  }
};

export type T_KeyWordAutoAssignMessageBlock = T_BasicMessageBlock & {
  type: 505
  message: string
  extension: {
    groupId: string
    ruleId: string
  }
};

export interface T_TemplateButton {
  button_id: string
  type: 'postback' | 'url' | 'uri' | 'system'
  title: string
  uuid?: string
  url?: string
  tags: string[]
  saveAttr?: {
    key: string
    value: string
    type: (typeof E_AttributeTypes)[keyof typeof E_AttributeTypes]
  }
  subtype?: string // required when type is system
}

// Include the text message type (301)
export type T_ButtonTemplateMessageBlock = T_BasicMessageBlock & {
  type: 201 | 301
  message: string
  extension: {
    buttons: T_TemplateButton[]
    title: string
  }
};
export type T_TextMessageBlock = T_ButtonTemplateMessageBlock;

export type T_CarouselTemplateMessageBlock = T_BasicMessageBlock & {
  type: 202 | 203
  message: string
  imageAspectRatio: 'square' | 'rectangle'
  extension: {
    imageUrl: string
    title: string
    text: string
    defaultAction: {
      type?: 'uri'
      title: string
      url?: string // required if defaultAction is not null
      tags: string[]
    }
    buttons: T_TemplateButton[]
  }[]
};

export type T_MessageBlock = (
  | T_BasicMessageBlock
  | T_InvoiceMessageBlock
  | T_TextMessageBlock
  | T_ImageMessageBlock
  | T_ImageMapMessageBlock
  | T_VideoMessageBlock
  | T_CouponMessageBlock
  | T_ImageCarouselMessageBlock
  | T_WhatsappStickerPackMessageBlock
  | T_ImageMapV2MessageBlock
  | T_WhatsappFlowMessageBlock
  | T_QuietReplyMessageBlock
  | T_UserInputMessageBlock
  | T_JSONApiMessageBlock
  | T_DelayedReplyMessageBlock
  | T_ConditionMessageBlock
  | T_KeyWordAutoAssignMessageBlock
  | T_ButtonTemplateMessageBlock
  | T_CarouselTemplateMessageBlock
);
```

---
layout: image
image: /Typing Effect.png
backgroundSize: contain
---

---
layout: image
image: /draggableDemo.gif
backgroundSize: auto 30vh
---
# Draggable
使用 [SortableJS-vue3](https://github.com/maxleiter/sortablejs-vue3)
---
layout: two-cols
---
### Basic Usage

```vue
<template>
  <Draggable
    v-slot="{ bind }"
    v-model="cardList"
    item-key="id"
    trigger="icon"
  >
    <Card v-bind="bind"></Card>
  </Draggable>
</template>
```
```vue
<slot :bind="returningElement(index, element)" />
```
::right::

### Props & Binds
```ts
interface I_Props {
  trigger?: 'icon' | 'all' | 'handle';
  // must give item key or the items order won't be correct
  itemKey: string | ((item: any) => string | number | Symbol) | undefined;
  handleClassName?: string;
  noHandle?: boolean;
  spacing?: 'compact' | 'normal' | 'none';
  horizontal?: boolean;
}
```
```ts
const returningElement: (_index: number, element: T) => T_BindingElement = (_index, element: any) => ({
  isGrabbing: isGrabbing.value,
  grabbingIndex: currentDraggedIndex.value,
  ...element,
  idx: _index,
  index: _index + 1,
});
```
---

# examples

<div class="flex">
  <img src="/draggableDemo.gif" class="w-[300px]" >
  <img src="/btnDrag.gif" class="w-[300px]" >
  <img src="/carouselDrag.gif" class="w-[300px]" >
</div>
---
layout: image
image: /collapsibleDemo.gif
backgroundSize: auto 30vh
---

# Collapsible

---
layout: two-cols-header
---
### Basic Usage

::left::
```vue
<Collapsible>
  content
</Collapsible>
```

::right::

```ts
interface I_Props {
  index?: number | string
  icon?: string
  expand?: boolean
  title?: string
  message?: string
  isGrabbing?: boolean
  grabbingIndex?: number
  maxWidth?: string | number
  width?: string | number
  noHeader?: boolean
  forceOpen?: boolean
  model?: any
  hasError?: boolean
  errorMessage?: string
  hasAddPrevCard?: any
}
```

---

### Slots

<div class="flex items-center justify-around h-full">
  <img class="h-full" src="/collapseSlots.png" />
</div>
---

```ts {*}{maxHeight: '100%'}
// in <SettingPanel.vue />
const {
  collapsibleList,
  expandAllCards,
  collapseAllCards
} = useCollapsibleController(nodeId);
// Provide collapsibleList to Collapsible components
provide('collapsibleList', collapsibleList);

// in <Collapsible />
const collapsibleList = inject('collapsibleList') as Ref;
onMounted(() => {
  collapsibleList?.value.push({
    isOpen,
    validate
  });
})

// in `useCollapsibleController`
const updateCollapsibleListValue = (status: boolean) => {
  collapsibleList.value.forEach((item) => {
    item.isOpen = status;
  });
};

const expandAllCards = () => {
  updateCollapsibleListValue(true);
};

const collapseAllCards = () => {
  updateCollapsibleListValue(false);
};
```

---
layout: center
---

# Drawer Resizer

---
layout: center
---

# Validatable (didn't be done 🥲)

---

```vue {all|10,11}
<TextareaEditor
  :model-value="modelValue.title"
  :render-list="['emoji']"
  class="input_editor"
  :placeholder="t('BOT_BUILDER.KEY129')"
  :maxlength="80"
  show-count
  counter-position="bottom"
  wrapper-class="error_hint_absolute"
  :name="isExpanded ? ['title'] : ['extension', modelValue.idx, 'title']"
  :rules="[{ required: true, message: t('THIS_FIELD_IS_REQUIRED'), trigger: ['blur', 'change'] }]"
  :initial-height="74"
  @update:model-value="emit('update:modelValue', { ...modelValue, title: $event })"
/>
```

---
layout: two-cols
---

```vue {all|14}
<template>
  <FormItem
    :name="['extension', currentSlideIdx, 'buttons', currentEditBtn!.idx, 'url']"
    :rules="[
      {
        required: true,
        message: t('THIS_FIELD_IS_REQUIRED'),
        trigger: ['change', 'blur'],
      },
      {
        required: false,
        message: t('BOT_BUILDER.KEY152'),
        trigger: ['change', 'blur'],
        validator: urlValidator,
      }]"
    class="w-full"
    >
    <TextField
      v-model="currentEditBtn!.url"
      :placeholder="t('BOT_BUILDER.KEY271')"
    />
  </FormItem>
</template>
```

::right::

```vue {all|3-6}
<script setup lang="ts">
const urlValidator = (rule: any, value: string) => {
  if (!value) {
    return Promise.resolve();
  }

  if (!isURLRegex.test(value)) {
    return Promise.reject(t('BOT_BUILDER.KEY152'));
  }
  return Promise.resolve();
};
</script>
```

---

## Expect to be

```vue
<template>
  <ValidatableRoot ref="ref_validatableRoot">

    <Validatable
      v-model="value"
      :isValidate="isValidate"
      :rules="[(v) => !!v || 'This is required']"
    >
      input type stuff or any content
    </Validatable>

  </ValidatableRoot>
</template>

<script setup lang="ts">
const ref_validatableRoot = ref();

const doValidate = () => {
  const { isValid, errors, fields } = ref_validatableRoot.value?.validate();

  // do something
}
</script>
```
---

## Example (input type)

```vue
<template>

  <Validatable
    v-model="value"
    :isValidate="isValidate"
    :rules="[(v) => !!v || 'This is required']"
  >
    <input v-model="value" />
  </Validatable>

</template>
```

![](/inputErrorHint.png)
---
layout: two-cols
---

## Example (any content)

```vue 
<template>

  <Validatable
    v-model="value"
    :isValidate="isValidate"
    :rules="[validator]"
    hide-hint
  >
    <button :class="{ 'border-danger': !isValidate }">
      <TriangleAlertIcon v-if="!isValidate" />

      ButtonText
    </button>
  </Validatable>

</template>

<script setup lang="ts">
const validator = (v) => {
  if (someCondition) {
    return true;
  }
  return 'Something is wrong';
}
</script>
```

::right::
<div class="flex justify-center items-center h-full p-10">
  <img src="/buttonErrorHint.png" />
</div>
---
layout: end
---
# Fin
