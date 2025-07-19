<style>
    .waiverTransaction {
        display: flex;
        flex-direction: column;
        margin-bottom: 1em;
    }

    .name {
        position: relative;
    }

    .core {
        display: flex;
        flex-direction: column;
        border-radius: 0 0 0 40px;
        border: 1px solid var(--ddd);
        border-left: 2px solid var(--blueTwo);
        border-bottom: none;
        background-color: var(--fff);
    }

    .avatarAndDetails {
        padding: 1em 0 0;
    }

    .avatar {
        position: absolute;
        left: 0px;
        top: 6px;
        border-radius: 50%;
        height: 40px;
        width: 40px;
        border: 2px solid var(--blueTwo);
        background-color: var(--fff);
    }

    .ownerName {
        display: inline-block;
        border-bottom: 2px solid var(--blueTwo);
        margin: 0 0 0 22px;
        padding-right: 30px;
        padding-left: 30px;
    }

    .details {
        display: flex;
        justify-content: center;
        align-items: flex-start;
        gap: 32px;
        flex-wrap: wrap;
        width: 100%;
        padding: 1em 2em;
    }

    .player {
        display: flex;
        flex-direction: column;
        align-items: center;
        width: 100px;
        position: relative;
    }

    .playerAvatar {
        position: relative;
        height: 60px;
        width: 60px;
        border-radius: 50%;
        border: 2px solid;
        background-position: center;
        background-repeat: no-repeat;
        background-size: cover;
        margin-bottom: 0.3em;
    }

    .indicator {
        position: absolute;
        bottom: -6px;
        right: -6px;
        font-size: 18px;
    }

    .nameHolder {
        display: flex;
        flex-direction: column;
        align-items: center;
        text-align: center;
    }

    .playerName {
        font-size: 0.8em;
        line-height: 1em;
        text-align: center;
    }

    .playerInfo {
        font-size: 0.6em;
        color: var(--g555);
        line-height: 1em;
    }

    .add {
        color: #00ceb8;
    }

    .drop {
        color: #ff2a6d;
    }

    .bid {
        color: var(--g555);
        font-style: italic;
    }

    .date {
        color: var(--g999);
        font-style: italic;
        font-size: 0.7em;
        text-align: center;
        margin-top: 0.7em;
    }

    .currentOwner {
        font-style: italic;
        color: var(--aaa);
    }

    .clickable {
        cursor: pointer;
    }

    @media (max-width: 410px) {
        .details {
            width: 90%;
            padding: 0 5%;
        }

        .player {
            width: 100%;
        }

        .nameHolder {
            margin-top: 0.5em;
            font-size: 0.9em;
        }
    }
</style>
