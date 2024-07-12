## Next.js 14 Server Side App with Server Actions, Infinite Scroll & Framer Motion Animations

This project leverages Next.js 14 to build a server-side rendered application featuring Server Actions for backend logic, Infinite Scroll for dynamic data loading, and Framer Motion for smooth animations.

### Server Actions

Next.js 14 introduces Server Actions to seamlessly integrate server-side logic into the application flow. In the provided example, `fetchAnime` function in `action.ts` demonstrates fetching anime data from the Shikimori API (Application Programming Interface) server-side:

```typescript
"use server";

import { AnimeProp } from "@/components/AnimeCard";

export const fetchAnime = async (page: number) => {
  const response = await fetch(
    `https://shikimori.one/api/animes?page=${page}&limit=8&order=popularity`
  );
  const data = await response.json();

  return data.map((item: AnimeProp, index: number) => (
    <AnimeCard key={item.id} anime={item} index={index} />
  ));
};
```

### Infinite Scroll

The `LoadMore.tsx` component showcases Infinite Scroll functionality using `react-intersection-observer` to trigger data fetching as the user scrolls:

```tsx
import { useEffect, useState } from "react";
import { useInView } from "react-intersection-observer";
import { fetchAnime } from "@/app/action";
import AnimeCard from "./AnimeCard";

let page = 2;

function LoadMore() {
  const { ref, inView } = useInView();
  const [data, setData] = useState<JSX.Element[]>([]);

  useEffect(() => {
    if (inView) {
      fetchAnime(page).then((res) => {
        setData((prevData) => [...prevData, ...res]);
        page++;
      });
    }
  }, [inView]);

  return (
    <>
      <section className="grid lg:grid-cols-4 md:grid-cols-3 sm:grid-cols-2 grid-cols-1 gap-10">
        {data}
      </section>

      <section className="flex justify-center items-center w-full">
        <div ref={ref}>
          <img
            src="/spinner.svg"
            alt="spinner"
            width={56}
            height={56}
            className="object-contain"
          />
        </div>
      </section>
    </>
  );
}

export default LoadMore;
```

### Framer Motion Animations

The `AnimeCard.tsx` component employs Framer Motion to animate the appearance of anime cards with smooth transitions:

```tsx
import Image from "next/image";
import { MotionDiv } from "./MotionDiv";

const variants = {
  hidden: { opacity: 0 },
  visible: { opacity: 1 },
};

function AnimeCard({ anime, index }: Prop) {
  return (
    <MotionDiv
      className="max-w-sm rounded relative w-full"
      variants={variants}
      initial="hidden"
      animate="visible"
      transition={{
        delay: index * 0.25,
        ease: 'easeInOut',
        duration: 0.5
      }}
    >
      {/* Anime card content */}
    </MotionDiv>
  );
}

export default AnimeCard;
```

`MotionDiv.tsx`

```tsx
"use client";

import { motion } from "framer-motion";

export const MotionDiv = motion.div;

```


### Summary

Next.js 14 enables building robust server-side rendered applications with integrated Server Actions for backend operations, Infinite Scroll for dynamic content loading, and Framer Motion for enhancing user interface interactions with smooth animations. This combination enhances both performance and user experience in modern web applications.