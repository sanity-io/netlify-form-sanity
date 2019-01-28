# How to use Netlify Forms and Functions to submit data to Sanity.io

This is a simple example of how you can use Forms and Functions in [Netlify](https://netlify.com) to submit data to your Sanity.io project.

![Demonstration of Netlify form via Functions to Sanity Studio](https://cdn.sanity.io/images/3do82whm/production/1728e9c2e1a25edbbb914e53edff8ded40ed2567-1642x1056.gif)

## How to use

1. Fork or clone this repo
2. Run `yarn` or `npm install`
3. Change the `projectId` and the `dataset` configuration in `/lambda/submission-created.js` to your Sanity.io project`s
4. Commit and push the changes to your GitHub repo
5. Connect Netlify to that repo, and add a environment variable in Netlify called `SANITY_TOKEN`
6. Go to your Settings -> API in your project at [manage.sanity.io](https://manage.sanity.io), and add a token with `write` rights.
7. Paste this token as the value for `SANITY_TOKEN` (be careful with where you store this token!)
8. Submissions will be stored as the `_type: "submission.form"`, and will not be available through the public API without a token with `read` rights.
9. If you want to view the submissions in the Studio, you can add the following schema to your project:

```js
/*
 * Doesn't cover all the data fields.
 * Remove or sest readOnly to `false` if you want to be able
 * to edit the responses in the Studio
 */

export default {
  name: 'submission.form',
  type: 'document',
  title: 'Form submission',
  readOnly: true,
  fields: [
    {
      name: 'title',
      type: 'string',
      title: 'Title'
    },
    {
      name: 'number',
      type: 'number',
      title: 'Number'
    },
    {
      name: 'created_at',
      type: 'datetime',
      title: 'Created at'
    },
    {
      name: 'data',
      type: 'object',
      title: 'Data',
      fields: [
        {
          name: 'email',
          type: 'email',
          title: 'Email'
        },
        {
          name: 'name',
          type: 'string',
          title: 'Name'
        },
        {
          name: 'message',
          type: 'text',
          title: 'Message'
        },
        {
          name: 'role',
          type: 'array',
          title: 'Role',
          of: [{ type: 'string' }]
        }
      ]
    }
  ]
}

```
