# Remix research

### Remix it's relatively new react library that provides SSR solution to your React application.

The main advantages about `Remix`, is that it lets you write code without having to worry about what's backend code, and what's frontend code (to a point, we'll talk about that in a minute). All you have to worry about is writing your routes, exporting your components, defining your loaders and that's it.
The reason why you get to work like that, is because `Remix` takes your code, and creates proxy modules, only using what they need for each environment.

---

### **Main features of Remix**

- **_Nested pages_**.<br />
  Any page inside a route folder is nested in the route instead of being separate. This means you can embed these components into your parent page, which also means less loading time.<br />
  Another advantage of doing this is that we can enforce error boundaries to these embedded pages, which will help with error handling.

_Project structure_

![](https://blog.logrocket.com/wp-content/uploads/2021/12/Remix-folder-structure.png)

- **_Error Handling_**<br />
  The way `Remix` handles errors is quite unique, as it allows us to create ErrorBoundarys that will be shown in case something with our route components didn't work as expected and an error is thrown. That way, if we're using `Remix's` nested routes, we might see a single
  throwing and error, but not necessarily the whole page. The smart thing about error boundaries is that they bubble up (the routes) to the closest error boundary. So the easiest case would be having one error boundary at the root level, comparable to a full 404. However, the image below nicely demonstrates how having multiple small error boundaries (one per route component for example) can leave the rest of an application intact.
  While error boundaries can be implemented in Next.js as well, `Remix` has this built in, and I think it’s a cool feature for production builds so that the user doesn’t get locked out of the entire page for a simple error.

```
export function ErrorBoundary({ error }) {
  return (
    <html>
      <head>
        <title>Something went wrong!</title>
        <Meta />
        <Links />
      </head>
      <body>
        ... anything we want to let the user know goes here
      </body>
    </html>
  );
}
```

![image](https://user-images.githubusercontent.com/41162650/181692504-171c6d9f-1b0d-45f2-99b2-36d3b802476c.png)

- **_Transitions_**<br />
  `Remix` automatically handles all loading states for you; all you have to do is tell `Remix` what to show when the app is loading. In other frameworks like Next.js, you need to set the loading state using some state management library like Redux or Recoil. While there are libraries that can help you do the exact same thing in other frameworks, `Remix` has this built in.

- **_Traditional forms_**<br />
  Now we are going back when developers used PHP. We used to specify a form method and action with a valid PHP URL; we use a similar approach in `Remix.`

- **_Data loading _**<br />
  To get data inside a route component in `Remix`, we can use a loader, which is just an async function that returns the requested data. Inside our components, we can then access it through a hook called useLoaderData.

```
import { useLoaderData } from "remix";

export let loader = async () => {
  return getData(); // does the heavy lifting, DB calls etc. and returns data
}

// Component function starts here
export default function Component() {
    let allData = useLoaderData(); // data is now available inside our component
}
```

- **_Storing data_**<br />
  If we want to send new user-generated data back to the backend, to save it to a database for example, `Remix` lets us use so-called actions. Actions rely on forms for the actual data input and are also only executed server-side, despite being in your route file. The functions are — again by convention — called action and can also trigger (return) a redirect. Let's look at an example.

```
export let action = async ({ request }) => {
  let formData = await request.formData()
  let title = formData.get("title")
  let slug = formData.get("slug")

  await createPost({ title, slug }) // actual call to store data...

  return redirect("/home")
}

export default function NewPost() {
  return (
    <Form method="post">
      <p>
        <label>Post Title: <input type="text" name="title" /></label>
      </p>
      <p>
        <label>Post Slug: <input type="text" name="slug" /></label>
      </p>
      <button type="submit">Create Post</button>
    </Form>
  );
```

We see that the action function takes the request as a parameter and thereby has access to everything our form sends over to the server. From there we're free to use any node code to store our data.

---

### **Remix disadvantages**

- **_Small Community_** <br/>
  `Remix` has only recently been co-opted, and so far not many people use it in projects.<br />So when you run into problems, it can be hard to find a solution online.

- **_Non-transparent routing system_** <br/>
  When I started working with `Remix`, the routing system confused me a bit. I couldn't understand the concept of nested routers. I had to use other frameworks without that specific system. That's why `Remix` has a slightly higher entry threshold.

---

## **How remix works**

<img width="1044" alt="image" src="https://user-images.githubusercontent.com/41162650/181686954-cb52f8aa-fab3-4fc9-b16f-1f220b333315.png">

Remix says that our code should run on the servers side. Our client side calls `loader()` function which always runs on the server. But how we can get our data from server to our `View` Component? Through props like in NextJs(another implementation SSR in React)? No, it implemented in another and better way. Only thing that you need to have it's hook `useLoaderData` that will fetch needed information from server to your client side.

Also `Remix` do important optimization with your requests to the server. It minimize quantity of these requests if on the client side it was called a couple times.<br/>
Example with bracket: If users clicks a couple times on product quantity, then server will receive only 1 last request.

---

## **Conclusion**

`Remix` 100% will find his community and place in project creating world, really like the way how it works, yes, for now it has not a big community, but if you need SSR in your project `Remix` will be a good solution, in compassion with `Next.js` in Remix with a minimum of effort we get a lot of features in the beginning that allow us to quickly start writing a project without a lot of hassle and thinking, which makes development faster than in Nodejs(for Juniors it also much easier to understand) On the image below shows that remix requires much less time to provide main features, but on the long distance to provide difficult things it would be harder than in `NextJs`

<img width="560" alt="image" src="https://user-images.githubusercontent.com/41162650/181695234-20c543f7-2ea9-4229-8560-96e55310d57e.png">

---

### **Resources**

> [Breaking Remix JS: A 'How Not To' Guide](https://blog.bitsrc.io/breaking-remix-js-a-how-not-to-guide-b2c931b900a3)<br />

> [Remix: A guide to the newly open-sourced React framework](https://blog.logrocket.com/remix-guide-newly-open-sourced-react-framework/)<br />

> [NextJs vs Remix](https://betterprogramming.pub/next-js-vs-remix-analyzing-key-aspects-and-differences-8674beaba695)<br />

> [Javascript Ninja Remix](https://www.youtube.com/watch?v=Xtkjf69lHp8)<br />
