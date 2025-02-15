---
title: 'Unit tests'
---

Teams test a variety of UI characteristics using different tools. Each tool requires you to replicate the same component state over and over. That’s a maintenance headache. Ideally, you’d set up your tests in the same way and reuse that across tools.

Storybook enables you to isolate a component and capture its use cases in a `*.stories.js|ts` file. Stories are standard JavaScript modules cross-compatible with the whole JavaScript ecosystem.

Stories are a practical starting point for UI testing. Import stories into tools like [Jest](https://jestjs.io/), [Testing Library](https://testing-library.com/), [Vitest](https://vitest.dev/) and [Playwright](https://playwright.dev/), to save time and maintenance work.

<IfRenderer renderer='vue'>

## Set up the testing addon

Storybook's [`@storybook/testing-vue3`](https://storybook.js.org/addons/@storybook/testing-vue3/) addon is a powerful tool that simplifies the testing process by allowing you to reuse your stories inside alongside their associated mocks, dependencies, and context, saving time and ensuring consistency and accuracy in the testing process.

Run the following command to install the addon.

<!-- prettier-ignore-start -->

<CodeSnippets
  paths={[
    'vue/storybook-testing-library-install.yarn.js.mdx',
    'vue/storybook-testing-library-install.npm.js.mdx',
    'vue/storybook-testing-library-install.pnpm.js.mdx',
  ]}
/>

<!-- prettier-ignore-end -->

<Callout variant="info">

If you're using Storybook 7 or higher, the `@storybook/testing-vue3` addon is the only one we support. For Vue 2 users, refer to the [troubleshooting section](#troubleshooting) for additional guidance.

</Callout>

</IfRenderer>

## Write a test with Testing Library

[Testing Library](https://testing-library.com/) is a suite of helper libraries for browser-based interaction tests. With [Component Story Format](../api/csf.md), your stories are reusable with Testing Library. Each named export (story) is renderable within your testing setup. For example, if you were working on a login component and wanted to test the invalid credentials scenario, here's how you could write your test:

<!-- prettier-ignore-start -->

<CodeSnippets
  paths={[
    'react/component-test-with-testing-library.js.mdx',
    'react/component-test-with-testing-library.ts.mdx',
    'vue/component-test-with-testing-library.3.js.mdx',
    'vue/component-test-with-testing-library.3.ts.mdx',
  ]}
/>

<!-- prettier-ignore-end -->

<Callout variant="info" icon="💡">

You can use Testing Library out-of-the-box with [Storybook Interaction Testing](./interaction-testing.md).

</Callout>

Once the test runs, it loads the story and renders it. [Testing Library](https://testing-library.com/) then emulates the user's behavior and checks if the component state has been updated.

## Configure

By default, Storybook offers a zero-config setup for React and other frameworks via addons, allowing you to run your stories as tests with Testing Library. However, if you're running tests and you've set up specific configurations in your Storybook instance (e.g., global [decorators](../writing-stories/decorators.md#global-decorators), [parameters](../writing-stories/parameters.md#global-parameters)) that you want to use in your tests, you'll need to extend your test setup to include these configurations. To do so, create a `setup.js|ts` file as follows:

<!-- prettier-ignore-start -->

<CodeSnippets
  paths={[
    'react/storybook-testing-addon-optional-config.js.mdx',
    'vue/storybook-testing-addon-optional-config.js.mdx',
  ]}
/>

<!-- prettier-ignore-end -->

<IfRenderer renderer='react'>

Update your test script to include the configuration file:

<!-- prettier-ignore-start -->

<CodeSnippets
  paths={[
    'react/react-test-scripts-optional-config-scripts.json.mdx',
  ]}
/>

<!-- prettier-ignore-end -->

</IfRenderer>

<IfRenderer renderer='vue'>

Update your test configuration file (e.g., `vite.config.js|ts`) if you're using [Vitest](https://vitest.dev/), or your test script if you're using [Jest](https://jestjs.io/):

<!-- prettier-ignore-start -->

<CodeSnippets
  paths={[
    'vue/storybook-testing-addon-optional-config.vite.js.mdx',
    'vue/storybook-testing-addon-optional-config.vite.ts.mdx',
    'vue/vue-jest-optional-config-scripts.jest.js.mdx',
    'vue/vue-jest-optional-config-scripts.jest.ts.mdx',
  ]}
/>

<!-- prettier-ignore-end -->

</IfRenderer>

### Override story properties

By default, the `setProjectAnnotations` function injects into your existing tests any global configuration you've defined in your Storybook instance (i.e., parameters, decorators in the `preview.js|ts` file). Nevertheless, this may cause unforeseen side effects for tests that are not intended to use these global configurations. To avoid this, you can override the global configurations by extending either the `composeStory` or `composeStories` functions to provide test-specific configurations. For example:

<!-- prettier-ignore-start -->

<CodeSnippets
  paths={[
    'react/override-compose-story-test.compose-story.js.mdx',
    'react/override-compose-story-test.compose-story.ts.mdx',
    'vue/override-compose-story-test.compose-story.3.js.mdx',
    'vue/override-compose-story-test.compose-story.3.ts.mdx',
    'react/override-compose-story-test.compose-stories.js.mdx',
    'react/override-compose-story-test.compose-stories.ts.mdx',
    'vue/override-compose-story-test.compose-stories.3.js.mdx',
    'vue/override-compose-story-test.compose-stories.3.ts.mdx',
  ]}
/>

<!-- prettier-ignore-end -->

## Run tests on a single story

You can use the `composeStory` function from the appropriate framework or supported addon to allow your tests to run on a single story. However, if you're relying on this method, we recommend that you supply the story metadata (i.e., the [default export](../writing-stories/introduction.md#default-export)) to the `composeStory` function. This ensures that your tests can accurately determine the correct information about the story. For example:

<!-- prettier-ignore-start -->

<CodeSnippets
  paths={[
    'react/single-story-test.js.mdx',
    'react/single-story-test.ts.mdx',
    'vue/single-story-test.3.js.mdx',
    'vue/single-story-test.3.ts.mdx',
  ]}
/>

<!-- prettier-ignore-end -->

## Combine stories into a single test

If you intend to test multiple stories in a single test, use the `composeStories` function from the appropriate framework or supported addon. The function will process every component story you've specified, including any [`args`](../writing-stories/args.md) or [`decorators`](../writing-stories/decorators.md) you've defined. For example:

<!-- prettier-ignore-start -->

<CodeSnippets
  paths={[
    'react/multiple-stories-test.js.mdx',
    'react/multiple-stories-test.ts.mdx',
    'vue/multiple-stories-test.3.js.mdx',
    'vue/multiple-stories-test.3.ts.mdx',
  ]}
/>

<!-- prettier-ignore-end -->

## Troubleshooting

### Run tests in other frameworks

Storybook provides community-led addons for other frameworks like [Vue 2](https://storybook.js.org/addons/@storybook/testing-vue) and [Angular](https://storybook.js.org/addons/@storybook/testing-angular). However, these addons still lack support for the latest stable Storybook release. If you're interested in helping out, we recommend reaching out to the maintainers using the default communication channels (GitHub and [Discord server](https://discord.com/channels/486522875931656193/839297503446695956)).

<IfRenderer renderer='react'>

### The args are not being passed to the test

The components returned by `composeStories` or `composeStory` not only can be rendered as React components but also come with the combined properties from the story, meta, and global configuration. This means that if you want to access args or parameters, for instance, you can do so:

<!-- prettier-ignore-start -->

<CodeSnippets
  paths={[
    'react/reuse-args-test.js.mdx',
    'react/reuse-args-test.ts.mdx',
  ]}
/>

<!-- prettier-ignore-end -->

</IfRenderer>

<IfRenderer renderer='vue'>

### The args are not being passed to the test

When using the `composeStories` or `composeStory` functions, the components being rendered will have a combination of properties from the story, meta, and global configuration. Therefore, if you need to access the args or parameters, you can do so as follows:

<!-- prettier-ignore-start -->

<CodeSnippets
  paths={[
    'vue/reuse-args-test.3.js.mdx',
    'vue/reuse-args-test.3.ts.mdx',
  ]}
/>

<!-- prettier-ignore-end -->

</IfRenderer>

#### Learn about other UI tests

- [Test runner](./test-runner.md) to automate test execution
- [Visual tests](./visual-testing.md) for appearance
- [Accessibility tests](./accessibility-testing.md) for accessibility
- [Interaction tests](./interaction-testing.md) for user behavior simulation
- [Coverage tests](./test-coverage.md) for measuring code coverage
- [Snapshot tests](./snapshot-testing.md) for rendering errors and warnings
- [End-to-end tests](./stories-in-end-to-end-tests.md) for simulating real user scenarios
- Unit tests for functionality
