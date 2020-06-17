---
title: Add more Blocks
---

<!-- Add more explanation here about the fields, settings, and ideas -->

![gif with new blocks added]()

## Add an _Images_ Block

At this point, we've got a taste of the many different aspects to configuring blocks, but our page is lacking — **we can only add one type of block**.

In this step we will add a few more block types: Image Diptych & Paragraph. Feel free to copy and paste these examples directly into the project.

Create a new file, `components/Images.js` and add this code:

**components/Images.js**

```jsx
import React from 'react'
import { BlocksControls, InlineImage } from 'react-tinacms-inline'
import '../styles/images.css'

export function Images({ index }) {
  return (
    <BlocksControls
      index={index}
      focusRing={{ offset: 0 }}
      insetControls={true}
    >
      <div className="wrapper">
        <div className="image-diptych">
          <InlineImage
            name="left.src"
            parse={filename => `${filename}`}
            uploadDir={() => '/'}
            previewSrc={formValues => `${formValues.blocks[index].left.src}`}
            focusRing={false}
          />
          <InlineImage
            name="right.src"
            parse={filename => `/${filename}`}
            uploadDir={() => '/'}
            previewSrc={formValues => `${formValues.blocks[index].right.src}`}
            focusRing={false}
          />
        </div>
      </div>
    </BlocksControls>
  )
}

export const images_template = {
  label: 'Image Diptych',
  defaultItem: {
    _template: 'images',
    left: {
      src: '/ivan-bandura-unsplash-square.jpg',
      alt: 'Some alt text',
    },
    right: {
      src: '/martin-sanchez-unsplash-square.jpg',
      alt: 'Some alt text',
    },
  },
  fields: [
    {
      name: 'left.src',
      label: 'Left-Hand Image',
      component: 'image',
      parse: filename => `/${filename}`,
      uploadDir: () => '/',
      previewSrc: () => '/ivan-bandura-unsplash-square.jpg',
      focusRing: false,
    },
    {
      name: 'left.alt',
      label: 'Left-Hand Image Alt Text',
      component: 'text',
    },
    {
      name: 'right.src',
      label: 'Right-Hand Image',
      component: 'image',
      parse: filename => `/${filename}`,
      uploadDir: () => '/',
      previewSrc: () => '/martin-sanchez-unsplash-square.jpg',
      focusRing: false,
    },
    {
      name: 'right.alt',
      label: 'Right-Hand Image Alt Text',
      component: 'text',
    },
  ],
}
```

<!-- Add note on accessing index from input for template previewSrc? -->

Since this project isn't set up with a [media store](https://tinacms.org/docs/media), uploading new images won't work. The type of media store you use depends on your back-end, and that is beyond the scope of this guide. So for our learning purposes, the same images will always render in the `Images` block.

## Add a _Paragraph_ Block

Create a new file, `components/Paragraph.js` and add this code:

<!-- Is this needed?  -->

**components/Paragraph.js**

```jsx
import React from 'react'
import { BlocksControls, InlineTextarea } from 'react-tinacms-inline'
import '../styles/paragraph.css'

export function Paragraph({ index }) {
  return (
    <BlocksControls
      index={index}
      focusRing={{ offset: 0 }}
      insetControls={true}
    >
      <div className="paragraph__background">
        <div className="wrapper wrapper--narrow">
          <p className="paragraph__text">
            <InlineTextarea name="text" focusRing={false} />
          </p>
        </div>
      </div>
    </BlocksControls>
  )
}

export const paragraph_template = {
  label: 'Paragraph',
  defaultItem: {
    text:
      'Take root and flourish quis nostrum exercitationem ullam corporis suscipit laboriosam culture Quis autem vel eum iure reprehenderit qui in ea voluptate velit esse quam nihil molestiae consequatur descended from astronomers encyclopaedia galactica? Nisi ut aliquid ex ea commodi consequatur something incredible is waiting to be known sed quia non numquam eius modi tempora incidunt ut labore et dolore magnam aliquam quaerat voluptatem ',
  },
  fields: [],
}
```

## Update the source data

**data/data.json**

```json
{
  "blocks": [
    {
      "_template": "hero",
      "background_color": "rgb(5, 30, 38)",
      "text_color": "#fffaf4",
      "headline": "Suspended in a Sunbeam",
      "subtext": "Dispassionate extraterrestrial observer are creatures of the cosmos courage of our questions inconspicuous motes of rock and gas a mote of dust suspended in a sunbeam great turbulent clouds.",
      "align": "center"
    },
    {
      "_template": "images",
      "left": {
        "src": "/ivan-bandura-unsplash-square.jpg",
        "alt": "Some alt text"
      },
      "right": {
        "src": "/martin-sanchez-unsplash-square.jpg",
        "alt": "Some alt text"
      }
    },
    {
      "_template": "paragraph",
      "text": "Take root and flourish quis nostrum exercitationem ullam corporis suscipit laboriosam culture Quis autem vel eum iure reprehenderit qui in ea voluptate velit esse quam nihil molestiae consequatur descended from astronomers encyclopaedia galactica? Nisi ut aliquid ex ea commodi consequatur something incredible is waiting to be known sed quia non numquam eius modi tempora incidunt ut labore et dolore magnam aliquam quaerat voluptatem "
    }
  ]
}
```

## Add new blocks to `Home`

And let's pass these blocks to our `InlineBlocks` component.

**Home.js**

```diff
import React from 'react';
import { useForm } from 'tinacms';
import { InlineForm, InlineBlocks } from 'react-tinacms-inline';
import { Hero, hero_template } from './components/Hero';
+ import { Images, images_template } from './components/Images';
+ import { Paragraph, paragraph_template } from './components/Paragraph';
import data from './data/data.json';

export default function Home() {
  //...
}

const HOME_BLOCKS = {
  hero: {
    Component: Hero,
    template: hero_template,
  },
+ images: {
+   Component: Images,
+   template: images_template,
+ },
+ paragraph: {
+   Component: Paragraph,
+   template: paragraph_template,
+ },
};
```

[👋 Checkout Step 6]()