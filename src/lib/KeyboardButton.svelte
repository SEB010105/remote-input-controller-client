<script lang="ts">

    import {post} from "./publicFunctions";
    import {address} from "./adressStore";
    import {Key} from "./key";

    export let key: Key;

    function handleDown() {
        if (key.toggleable) {
            post(toggledPostAddress, {key: key.currentKey});
            key.toggle();
        } else {
            post($address + "/down", {key: key.currentKey});
        }
    }

    function handleUp() {
        if (!key.toggleable)
            post($address + "/up", {key: key.currentKey});
    }

    let styling: string;
    let content: string;
    let toggledPostAddress: string;

    key.toggled.subscribe(t => {
        styling = t ? "bg-gray-700" : "bg-gray-600";
        toggledPostAddress = $address + (t ? "/up" : "/down");

        if (key.toggleFunctionality)
            key.toggleFunctionality(t);
    });

    key.content.subscribe(c =>
        content = c
    );
</script>

<button
        id={key.currentKey}
        class={styling}
        on:mousedown={handleDown}
        on:mouseup={handleUp}
>{@html content}</button>

<style>
    button {
        outline: none;
        color: white;
        border-radius: 0.2rem;
        padding: 0.5rem;
        margin: 0.2rem;
        border-bottom: 0.3rem solid;
        border-bottom-color: theme("colors.gray.700");
        overflow: hidden;
        text-overflow: ellipsis;
        display: flex;
        align-items: center;
        justify-content: center;
        user-select: none;
        transition: 200ms;
    }

    @media (max-width: 700px) {
        button {
            padding: 0.3rem;
            font-size: 0.5rem;
        }
    }

    button:hover {
        background-color: theme("colors.gray.500");
        border-bottom-color: theme("colors.gray.600");
        transition: 100ms;
    }

    button:active {
        background-color: theme("colors.gray.700");
        border-bottom-color: theme("colors.gray.700");
    }

    #enter {
        grid-row: 4 / 6;
        grid-column: 14;
    }

    #space {
        grid-column: 5 / 11;
    }
</style>