# GetMoarFediverse

This is a small app that shows how you could use [FakeRelay](https://github.com/g3rv4/FakeRelay/) to import content into your instance that's tagged with hashtags you're interested in.

This doesn't paginate over the tags, that means it will import up to 20 statuses per instance. This also keeps a txt file with all the statuses it has imported.

## How can I run it?

### Download a prebuilt executable

You can download an executable for your environment [on the releases page](https://github.com/g3rv4/GetMoarFediverse/releases). In addition to that, you will need to set up a `config.json` file with your desired logic. It could be something like this:

```
{
    "FakeRelayUrl": "https://fakerelay.gervas.io",
    "FakeRelayApiKey": "1TxL6m1Esx6tnv4EPxscvAmdQN7qSn0nKeyoM7LD8b9mz+GNfrKaHiWgiT3QcNMUA+dWLyWD8qyl1MuKJ+4uHA==",
    "Tags": [ "dotnet", "csharp" ],
    "Instances": [ "hachyderm.io", "mastodon.social" ]
}
```

Once you have that file, all you need to run is `./GetMoarFediverse /path/to/config.json`. You can put it on a cron like this :)

```
1,16,31,46 * * * * /path/to/GetMoarFediverse /path/to/config.json > /path/to/GetMoarFediverse/cron.log 2>&1
```

You will find an executable for Windows as well, which you can use on a scheduled task.

### Have it download all the followed hashtags of your instance

The previous approach works, but you need to hardcode the list of tags. That's not awesome, as you need to keep track of the hashtags you (and your other users follow).

You can pass `MastodonPostgresConnectionString` with a connection string to your postgres database and GetMoarFediverse will download content for all the hashtags the users on your server follow. Here's an example:

```
{
    "FakeRelayUrl": "https://fakerelay.gervas.io",
    "FakeRelayApiKey": "1TxL6m1Esx6tnv4EPxscvAmdQN7qSn0nKeyoM7LD8b9m+GNfrKaHiWgiT3QcNMUA+dWLyWD8qyl1MuKJ+4uHA==",
    "MastodonPostgresConnectionString": "Host=myserver;Username=mastodon_read;Password=password;Database=mastodon_production",
    "Instances": [ "hachyderm.io", "mastodon.social" ]
}
```

I wrote a quick article [here](https://g3rv4.com/2022/12/getmoarfediverse-on-multi-user-instances) about it, with instructions to create a read only user.

### You can run it on docker

If that's what you like, you can check out [this document](docs/docker.md) with instructions.
