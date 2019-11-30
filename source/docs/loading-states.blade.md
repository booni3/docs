---
title: Loading States
extends: _layouts.documentation
section: content
---

Because Livewire makes a roundtrip to the server every time an action is triggered on the page, there are cases when the page may not react immediately to a user event (like a click). It is up to you to determine when you should provide the user with some kind of loading state or not.

## Toggling elements during "loading" states {#toggling-elements}

Elements with the `wire:loading` directive are only visible while waiting for actions to complete (network requests).

@code(['lang' => 'html'])
<div>
    <button wire:click="checkout">Checkout</button>

    <div wire:loading>
        Processing Payment...
    </div>
</div>
@endcode

When the "Checkout" button is clicked, the "Processing Payment..." message will show. When the action is finished, the message will disapear.

## Targeting specific actions {#targeting-actions}
The method outlined above works great for simple components, however, it's common to want to only show loading indicators for specific actions. Consider the following example:

@code(['lang' => 'html'])
<div>
    <button wire:click="checkout">Checkout</button>
    <button wire:click="cancel">Cancel</button>

    <div wire:loading>
        Processing Payment...
    </div>
</div>
@endcode

Notice, we've added a "Cancel" button to the checkout form. If the user clicks the "Cancel" button, the "Processing Payment..." message will show briefly. This is clearly undesirable, therefore Livewire offers two directives. You can add `wire:target` to the loading indicator, and pass in the name of a `ref` you define by attaching `wire:ref` to the target. Let's look at the adapted example:

@code(['lang' => 'html'])
<div>
    <button wire:click="checkout" wire:ref="checkout-button">Checkout</button>
    <button wire:click="cancel">Cancel</button>

    <div wire:loading wire:target="checkout-button">
        Processing Payment...
    </div>
</div>
@endcode

Now, when the "Checkout" button is clicked, the loading indicator will load, but not when the "Cancel" button is clicked.

To isolate loading state to an individual component `wire:loading` and `wire:ref` can be used together. In the below example we have some buttons that each trigger their own long running background process. When you click a button, only that button will trigger its loading state.

@code(['lang' => 'html'])
```
<div>
    <button wire:click="executeProcess(1)" wire:ref="process-btn-1" wire:target="process-btn-1" wire:loading.class="bg-red">
        Run 1
    </button>
    <button wire:click="executeProcess(2)" wire:ref="process-btn-2" wire:target="process-btn-2" wire:loading.class="bg-red">
        Run 2
    </button>
    <button wire:click="executeProcess(3)" wire:ref="process-btn-3" wire:target="process-btn-3" wire:loading.class="bg-red">
        Run 3
    </button>
    <button wire:click="executeProcess(4)" wire:ref="process-btn-4" wire:target="process-btn-4" wire:loading.class="bg-red">
        Run 4
    </button>
</div>
```
@endcode

Also note that `wire:target` can accept multiple `ref` arguments in a comma separated format like this: `wire:target="foo, bar"`.

## Toggling classes {#toggling-classes}

You can add or remove classes from an element during loading states, by adding the `.class` modifier to the `wire:loading` directive.

@code(['lang' => 'html'])
<div>
    <button wire:click="checkout" wire:loading.class="bg-gray">
        Checkout
    </button>
</div>
@endcode

Now, when the "Checkout" button is clicked, the background will turn gray while the network request is processing.

You can also perform the inverse and remove classes by adding the `.remove` modifier.

@code(['lang' => 'html'])
<div>
    <button wire:click="checkout" wire:loading.class.remove="bg-blue" class="bg-blue">
        Checkout
    </button>
</div>
@endcode

Now the `bg-blue` class will be removed from the button while loading.

## Toggling attributes {#toggling-attributes}

Similar to classes, HTML attributes can be added or removed from elements during loading states:

@code(['lang' => 'html'])
<div>
    <button wire:click="checkout" wire:loading.attr="disabled">
        Checkout
    </button>
</div>
@endcode

Now, when the "Checkout" button is clicked, the `disabled="true"` attribute will be added to the element while loading.
