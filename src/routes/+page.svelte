<script lang="ts">
    import { goto } from '$app/navigation';
    import ndkStore from '$lib/stores/ndk';
    import { Kinds } from '$lib/utils/constants';
    import { NDKEvent } from '@nostr-dev-kit/ndk';
    import { get } from 'svelte/store';
    import {Button} from '@getalby/bitcoin-connect';


    const ndk = get(ndkStore);
    let name = 'test';

    async function createRoom() {
        try {
            await ndk.assertSigner();
        } catch (error) {
            alert('log in first jackass');
            return;
        }
        const ev = new NDKEvent(ndk);
        ev.kind = Kinds.NEST_INFO;
        ev.tags = [
            ['alias', name],
            ['d', name],
            ['moderator', 'c5fb6ecc876e0458e3eca9918e370cbcd376901c58460512fe537a46e58c38bb'],
            ['audio_server', 'https://nostrnests.com']
        ];
        await ev.publish();
        // encodes to naddr style
        goto(`/r/${ev.encode()}`);
    }
</script>

<div class="flex flex-col items-center w-1/2 mx-auto text-center prose prose-invert">
    <h1 class="text-3xl font-extrabold mb-0">Nostr Nests</h1>
    <img src="/images/nostrich-nest.png" class="w-32 h-32 my-0" alt="Nostr Nests" />
    <div class="flex flex-col gap-8 items-center my-8">
        <input bind:value={name} />
        <button on:click={createRoom} class="button-primary">🪺 Start a Nest</button>
    </div>
    <p>
        <span class="font-bold">Nostr Nests</span> is an <span class="italic">audio space</span> for
        chatting, brainstorming, debating, jamming, micro-conferences and more.
    </p>
    <p>
        <a href="https://nostrplebs.com/" target="_blank">Nostr Plebs</a> provides services and solutions
        for the Nostr protocol and the greater Nostr ecosystem. Nostr Plebs is made by Bitcoiners, for
        Bitcoiners.
    </p>
    <br>
    <br>
     <p>
        Using <span class="font-bold">Nostr Nests</span> on a mobile browser? Connect your wallet using <span class="font-bold">Bitcoin Connect</span> 
        to enable one-click zaps!. Even if you are on your phone!
    </p>
     <bc-button></bc-button>
</div>
