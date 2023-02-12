---
title: "VueJS Amsterdam 2023"
---

# VueJS Amsterdam 2023

For the second time I joined the VueJS Amsterdam conference in person and it was a blast. I met so many great people and enjoyed really good talks.
Esspacialy the last talk of the conference named `Writing (Really) Good Tests`. 

It was a really interesting talk about **Writing test FIRST** and another one that I found really interesting **decouple your test from the framework** And that is what this blogpost is about. 

Currently I'm doing a online course about Vue3 and testing and I tried to decouple one of the written test fowlling the principles from the talk.

## The test as is

So in this test I'm testing if the user is logged in. When that is the case I should find a (for now) hardcoded number that represents the number of jobs found.

#### Setting up the test

```js
import { render, screen } from "@testing-library/vue";
import SubNav from "@/components/Navigation/SubNav.vue";

describe("SubNav", () => {
  const renderSubNav = (routeName) => {
    render(SubNav, {
      global: {
        stubs: {
          FontAwesomeIcon: true,
        },
        mocks: {
          $route: {
            name: routeName,
          },
        },
      },
    });
  };
```

Testing if the user is logged in and is on the jobs page

```js
  describe("when user is on jobs page", () => {
    it("displays job count", () => {
      renderSubNav("JobResults");

      const jobCount = screen.getByText("1423");
      expect(jobCount).toBeInTheDocument();
    });
  });
```

Testing if the user is not logged in and on the jobs page
```js
  describe("when user is not on jobs page", () => {
    it("does not display job count", () => {
      renderSubNav("Home");

      const jobCount = screen.queryByText("1423");
      expect(jobCount).not.toBeInTheDocument();
    });
  });
```