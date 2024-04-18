---
# try also 'default' to start simple
theme: seriph
# random image from a curated Unsplash collection by Anthony
# like them? see https://unsplash.com/collections/94734566/slidev
background: https://cover.sli.dev
# some information about your slides, markdown enabled
title: 1DS-QE and Signing Scenarios
info: |
  ## Slidev Starter Template
  Presentation slides for developers.

  Learn more at [Sli.dev](https://sli.dev)
# apply any unocss classes to the current slide
class: text-center
# https://sli.dev/custom/highlighters.html
highlighter: shiki
# https://sli.dev/guide/drawing
drawings:
  persist: false
# slide transition: https://sli.dev/guide/animations#slide-transitions
transition: slide-left
# enable MDC Syntax: https://sli.dev/guide/syntax#mdc-syntax
mdc: true
---

---
layout: two-cols
layoutClass: gap-16
---

# Intro

Intro to 1ds-qe and Scenarios

<Toc v-click minDepth="2" maxDepth="3"></Toc>

---
---

## Scenarios
<p style="font-size: .75em">
Scenarios are a custom utility that the sign team came up with for encapsulating all "Setup" logic into a playwright `Fixture`. Fixtures are used to write re-usable composable utilites for a test, and they are passed as the first argument to the test.
</p>

````md magic-move
```ts
// Start with a Blank test
import { setTestOwner, tagE2E, testE2E, expect } from '@App/Test/utils';

testE2E(tagE2E('my test'), async ({ page }) => {
});
```

```ts {*|6-19}
// Implement the setup logic
import { setTestOwner, tagE2E, testE2E, expect, createTabDefinition } from '@App/Test/utils';
import { apiWorkflows } from '@1ds/qe';

testE2E(tagE2E('my test'), async ({ page, userFactory }) => {
    const emailSubject = apiWorkflows.eSign.defaultData.envelope.defaultEmailSubject;

    const sender = await userFactory.createOrGetUser();
    const signer = await userFactory.createOrGetUser();

    const tab = createTabDefinition('signHereTabs', { height: '50', width: '50', }); // utility function places tabs

    /* Send an envelope to a signer with certain tabs */
    await apiWorkflows.eSign.envelope.sendEnvelopeToUser(
      signer,
      sender,
      { emailSubject },
      { tabs: createRecipientTabsFromTabDefinitions([tab]) }
    );
});
```

```ts {*|16-20|2-15}
// Implement the setup logic
    const emailSubject = apiWorkflows.eSign.defaultData.envelope.defaultEmailSubject;

    const sender = await userFactory.createOrGetUser();
    const signer = await userFactory.createOrGetUser();

    const tab = createTabDefinition('signHereTabs', { height: '50', width: '50', }); // utility function places tabs

    /* Send an envelope to a signer with certain tabs */
    await apiWorkflows.eSign.envelope.sendEnvelopeToUser(
      signer,
      sender,
      { emailSubject },
      { tabs: createRecipientTabsFromTabDefinitions([tab]) }
    );
    /* Act */
    await sign.openEnvelope(accessInfo);
    /* Assert */
    await expect(sign.pages.ersdDialog.ersdDialogLocators.title).toBeVisible();
});
```

```ts {*}
// Extract setup to scenario `__tests__/utils/scenarios/SingleSignerOneSignTab.ts`
import { apiWorkflows } from '@1ds/qe';
import { ScenarioFixtures, createTabDefinition } from '@App/Test/utils';
export async function SingleSignerOneSignTab({ userFactory, sign}: ScenarioFixtures ) => {
    const emailSubject = apiWorkflows.eSign.defaultData.envelope.defaultEmailSubject;

    const sender = await userFactory.createOrGetUser();
    const signer = await userFactory.createOrGetUser();

    const tab = createTabDefinition('signHereTabs', { height: '50', width: '50', }); // utility function places tabs

    /* Send an envelope to a signer with certain tabs */
    await apiWorkflows.eSign.envelope.sendEnvelopeToUser(
      signer,
      sender,
      { emailSubject },
      { tabs: createRecipientTabsFromTabDefinitions([tab]) }
    );
};
```
```ts {*|6-19}
// back to our test function
import { setTestOwner, tagE2E, testE2E, expect, createTabDefinition } from '@App/Test/utils';
import { apiWorkflows } from '@1ds/qe';

testE2E(tagE2E('my test'), async ({ page, userFactory }) => {
    const emailSubject = apiWorkflows.eSign.defaultData.envelope.defaultEmailSubject;

    const sender = await userFactory.createOrGetUser();
    const signer = await userFactory.createOrGetUser();

    const tab = createTabDefinition('signHereTabs', { height: '50', width: '50', }); // utility function places tabs

    /* Send an envelope to a signer with certain tabs */
    await apiWorkflows.eSign.envelope.sendEnvelopeToUser(
      signer,
      sender,
      { emailSubject },
      { tabs: createRecipientTabsFromTabDefinitions([tab]) }
    );
    /* Act */
    await sign.openEnvelope(accessInfo);
    /* Assert */
    await expect(sign.pages.ersdDialog.ersdDialogLocators.title).toBeVisible();
});
```

```ts {*|4|5-8}
// Replace the setup code with a call to the scenario
import { setTestOwner, tagE2E, testE2E, expect } from '@App/Test/utils';
testE2E(tagE2E('my test'), async ({ sign, scenarios }) => {
    const { accessInfo, signer } = await scenarios.SingleSignerOneSignTab();
    /* Act */
    await sign.openEnvelope(accessInfo);
    /* Assert */
    await expect(sign.pages.ersdDialog.ersdDialogLocators.title).toBeVisible();
});
```
````

---
layout: two-cols
layoutClass: gap-4
--- 


```ts {*|6-19|20-24}
// Implement the setup logic
import { setTestOwner, tagE2E, testE2E, expect, createTabDefinition } from '@App/Test/utils';
import { apiWorkflows } from '@1ds/qe';

testE2E(tagE2E('my test'), async ({ page, userFactory }) => {
    const emailSubject = apiWorkflows.eSign.defaultData.envelope.defaultEmailSubject;

    const sender = await userFactory.createOrGetUser();
    const signer = await userFactory.createOrGetUser();

    const tab = createTabDefinition('signHereTabs', { height: '50', width: '50', }); // utility function places tabs

    /* Send an envelope to a signer with certain tabs */
    await apiWorkflows.eSign.envelope.sendEnvelopeToUser(
      signer,
      sender,
      { emailSubject },
      { tabs: createRecipientTabsFromTabDefinitions([tab]) }
    );
    /* Act */
    await sign.openEnvelope(accessInfo);
    /* Assert */
    await expect(sign.pages.ersdDialog.ersdDialogLocators.title).toBeVisible();
});
```

::right::

```ts {*|4|5-8}
// End result with scenario
import { setTestOwner, tagE2E, testE2E, expect } from '@App/Test/utils';
testE2E(tagE2E('my test'), async ({ sign, scenarios }) => {
    const { accessInfo, signer } = await scenarios.SingleSignerOneSignTab();
    /* Act */
    await sign.openEnvelope(accessInfo);
    /* Assert */
    await expect(sign.pages.ersdDialog.ersdDialogLocators.title).toBeVisible();
});
```

---
---

## Why Scenarios?


1. Cleanliness
2. Integration testing
3. Composability


---
---

### Why Scenarios? - 1. Cleanliness
- "arrange" Can get messy and detracts from the "act" on and "assert"
- Scenarios package up "arrange" code into a single encapsulated spot

---
---

### Why Scenarios? - 2. Integration Testing

- Integration tests mock out (or just completely skip) the entire server redirect chain and load up the app right away
- scenarios skip the setup code if we're playing back an integration test and persist custom mock data when recording the test

---
---

### Why Scenarios? - 3. Composability


- Scenarios have inputs and outputs - inputs can customize a scenarios behavior
- Examples:
  - Inputs have `senderConfig` - we can pass that in as an input to the base scenario to customize it
  - Do something to the envelope after creation - like complete the enevelope, or complete tabs but not finish it 
    - Our scenario can call the "basic envelope" scenario first to create it, and then it can go through and modify it 
    - as long as we exit the session, and open it again with the updated envelope data - an integration test will be able to skip all of that and open the envelope in the exact state that the test cares about
- Future: we would like to be able to avoid duplicate HARs and have one set of the "basic envelope" data and be able to just patch it with the data we care about (for example an `accountPlanSettings`) to reduce recording time and space