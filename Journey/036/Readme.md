<!-- This is a template you can use for quick progress days. It removes a lot of the steps we encourage you to share in the longer template 000-DAY-ARTICLE-LONG-TEMPLATE.MD-->

# DevOps - The Introduction Course: 1

## Introduction to Chef

### Configuration management tools: Push vs Pull (Cook vs Buffet):

- Configuration `push` is initiated with the master node (from 'cook' to 'servers'), e.g. Ansible, Saltstack
- `pull` - you can think of it as a 'buffet', where the customer periodically walks up to the table and takes what they need, e.g. Puppet, Chef

### Idempotency & Convergence

Generic concepts used by operating systems and other orchestration tools

- An idempotent task is one that yields the same result when repeated multiple times

  - e.g. asking a waiter to fill your glass of water: whether it's empty, half-full or already full (requiring no action), you'll always end up with a full glass of water
  - e.g. in user creation, if the user doesn't exist then the user is created; if they _do_, then the operation is skipped:

  ```ruby
      user 'sam' do
        action :create
      end
  ```

- Convergence means "to bring together"

  - e.g bringing together a set of ingredients to cook a dish. The dish requires noodles - if they're already cooked, skip them; otherwise, cook noodles. While adding salt, taste to see if there's enough already before adding more.
  - What's important is that at the end of the process we get the same dish we're expecting that tastes the same, with the right amount of ingredients.

- Summary
  - No complex scriptting
  - Consistent in heterogenous environment
  - Imperative and declarative patterns
  - Idempotent

## Cloud Research

- SSH'd into an Ubuntu VM
- Revised YAML
- Learned about automation in Chef
  - Imperative vs Declaritve programming: is declarative == abstraction? Apparently, [yes](https://zach-gollwitzer.medium.com/imperative-vs-declarative-programming-procedural-functional-and-oop-b03a53ba745c#:~:text=Imperative%20programming%20is%20about%20how%2C%20and%20is%20where%20you%20list,It%20is%20the%20ultimate%20abstraction.)!
- Read a chapter of 'The Phoenix Project')

## Social Proof

[Twitter](https://twitter.com/_notwaving/status/1334228097713967105?s=20)
