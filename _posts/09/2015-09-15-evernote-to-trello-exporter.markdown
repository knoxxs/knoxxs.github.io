---
layout: post
title:  Evernote to trello exporter
date:   2015-09-15 04:10:00
categories: programming utility
tags: python evernote trello py-trello export html2text
---

We were using evernote to keep our startup ideas. Today we decided to use trello for the same as it has many more things to give: comments, lists, assignment etc. There is no direct way to shift our current list of ideas to trello. So I decided to do it programmatically. Here are the steps:

## Getting data from evernote
Evernote has a [quick starter guide](https://dev.evernote.com/doc/start/python.php) for the same.

1. The first thing we need it to install the python wrapper for evernote lib: `pip install evernote`
2. Create new empty python project with intellij.
3. Get dev tokens:
    1. [Sandbox](https://sandbox.evernote.com/api/DeveloperToken.action)
    2. [Production](https://www.evernote.com/api/DeveloperToken.action)
4. Write basic code to connect and get the evernote client as defined in the [sample code by evernote guys](https://github.com/evernote/evernote-sdk-python/blob/master/sample/client/EDAMTest.py)

    ```python
    import evernote.edam.userstore.constants as UserStoreConstants
    import evernote.edam.type.ttypes as Types

    from evernote.api.client import EvernoteClient

    # Real applications authenticate with Evernote using OAuth, but for the
    # purpose of exploring the API, you can get a developer token that allows
    # you to access your own Evernote account. To get a developer token, visit
    # https://sandbox.evernote.com/api/DeveloperToken.action
    auth_token = "your developer token"

    if auth_token == "your developer token":
        print "Please fill in your developer token"
        print "To get a developer token, visit " \
            "https://sandbox.evernote.com/api/DeveloperToken.action"
        exit(1)

    # Initial development is performed on our sandbox server. To use the production
    # service, change sandbox=False and replace your
    # developer token above with a token from
    # https://www.evernote.com/api/DeveloperToken.action
    client = EvernoteClient(token=auth_token, sandbox=True)
    ```
5. Write code to fetch notes inspired by [this SO answer](http://stackoverflow.com/questions/20080414/read-my-own-evernotes). To do this we first have to search the notebook by its name using [`findNotesMetadata`](https://dev.evernote.com/doc/reference/NoteStore.html#Fn_NoteStore_findNotesMetadata).To query notebook by name we need [search grammar](https://dev.evernote.com/doc/articles/search_grammar.php).

    ```python
    # get user store
        user_store = evernote_client.get_user_store()
        # check if API version is fine
        version_ok = user_store.checkVersion(
            "Evernote EDAMTest (Python)",
            user_store_constants.EDAM_VERSION_MAJOR,
            user_store_constants.EDAM_VERSION_MINOR
        )
        print "Is my Evernote API version up to date? ", str(version_ok)
        if not version_ok:
            exit(1)

        # get notes store
        note_store = evernote_client.get_note_store()
        # search for the notebook named ideas
        note_filter = NoteStore.NoteFilter()
        note_filter.words = 'notebook:"{}"'.format(EVERNOTE_NOTEBOOK_NAME)
        notes_metadata_result_spec = NoteStore.NotesMetadataResultSpec()
        notes_metadata_list = note_store.findNotesMetadata(note_filter, 0, NUM_EVERNOTE_NOTES, notes_metadata_result_spec)
        # get all the notes in the ideas notebook
        notes = {}
        print "Fetching notes..."
        for note_metadata in notes_metadata_list.notes:
            note_guid = note_metadata.guid

            # fetch complete note
            note = note_store.getNote(note_guid, True, True, False, False)
    ```

## Creating card in trello
Trello also have a well defined [api docs](https://trello.com/docs/).

1. Install [py-trello](https://github.com/sarumont/py-trello), the python wrapper of trello api: `pip install py-trello`
2. Get api keys: (Developer API Keys)[https://trello.com/1/appKey/generate]
3. Get oauth keys using the [util](https://github.com/sarumont/py-trello#getting-your-trello-oauth-token) defined in py-trello.

    ```python
    from trello import util

    api_key = 'xxxx'
    api_secret = 'xxxx'

    util.create_oauth_token(key=api_key, secret=api_secret)

    ```
4. Write code to get the trello client

    ```python
    # connect to trello
        trello_api_key = 'xxxx'
        trello_api_secret = 'xxxx'
        # this oauth token and secret are fetched using trello_oauth_util.py
        trello_oauth_token = "xxxx"
        trello_oauth_token_secret = "xxxx"
        trello_client = TrelloClient(api_key=trello_api_key, api_secret=trello_api_secret, token=trello_oauth_token,
                                     token_secret=trello_oauth_token_secret)
    ```
5. Write code to add a card

    ```python
    for board in trello_client.list_boards():
            if TRELLO_BOARD_NAME == board.name:
                for board_list in board.all_lists():
                    if TRELLO_LIST_NAME == board_list.name:
                        board_list.add_card("title", desc="content")
                break
    ```

## Converting html to markdown
Evernote returns description data in `html` while trello stores data in `markdown`. So I used [html2text](https://github.com/aaronsw/html2text) to convert `html` to `markdown`.

1. Install html2text: `pip install html2text`
2. Write the code to convert html to markdown.

    ```python
    import html2text
    print html2text.html2text("<p>Hello, world.</p>")
    ```

## Complete code

```python
__author__ = 'aapa'

import warnings

from trello import TrelloClient
import html2text

TRELLO_LIST_NAME = "Test"
TRELLO_BOARD_NAME = "Test"
EVERNOTE_NOTEBOOK_NAME = 'xxx'
NUM_EVERNOTE_NOTES = 2


def fetch_evernote_notes():
    global notes, title
    from evernote.edam.notestore import NoteStore
    from evernote.edam.userstore import constants as user_store_constants
    from evernote.api.client import EvernoteClient
    # Real applications authenticate with Evernote using OAuth, but for the
    # purpose of exploring the API, you can get a developer token that allows
    # you to access your own Evernote account. To get a developer token, visit
    # https://sandbox.evernote.com/api/DeveloperToken.action
    # sandbox
    # sandbox_auth_token = \
    #     "xxx"
    # Initial development is performed on our sandbox server. To use the production
    # service, change sandbox=False and replace your
    # developer token above with a token from
    # https://www.evernote.com/api/DeveloperToken.action
    # client = EvernoteClient(token=sandbox_auth_token, sandbox=True)
    # original
    production_auth_token = \
        "S=xxx"
    evernote_client = EvernoteClient(token=production_auth_token, sandbox=False)

    # get user store
    user_store = evernote_client.get_user_store()
    # check if API version is fine
    version_ok = user_store.checkVersion(
        "Evernote EDAMTest (Python)",
        user_store_constants.EDAM_VERSION_MAJOR,
        user_store_constants.EDAM_VERSION_MINOR
    )
    print "Is my Evernote API version up to date? ", str(version_ok)
    if not version_ok:
        exit(1)

    # get notes store
    note_store = evernote_client.get_note_store()
    # search for the notebook named ideas
    note_filter = NoteStore.NoteFilter()
    note_filter.words = 'notebook:"{}"'.format(EVERNOTE_NOTEBOOK_NAME)
    notes_metadata_result_spec = NoteStore.NotesMetadataResultSpec()
    notes_metadata_list = note_store.findNotesMetadata(note_filter, 0, NUM_EVERNOTE_NOTES, notes_metadata_result_spec)
    # get all the notes in the ideas notebook
    notes = {}
    print "Fetching notes..."
    for note_metadata in notes_metadata_list.notes:
        note_guid = note_metadata.guid

        # fetch complete note
        note = note_store.getNote(note_guid, True, True, False, False)
        title = note.title
        notes[title] = note.content

        if None != note.resources:
            print "resources are present for card: {}".format(title)

    return notes


def get_trello_list():
    # connect to trello
    trello_api_key = 'xxxx'
    trello_api_secret = 'xxxx'
    # this oauth token and secret are fetched using trello_oauth_util.py
    trello_oauth_token = "xxxx"
    trello_oauth_token_secret = "xxxx"
    trello_client = TrelloClient(api_key=trello_api_key, api_secret=trello_api_secret, token=trello_oauth_token,
                                 token_secret=trello_oauth_token_secret)
    # fetch the desired board
    # noinspection PyTypeChecker
    for board in trello_client.list_boards():
        if TRELLO_BOARD_NAME == board.name:
            for board_list in board.all_lists():
                if TRELLO_LIST_NAME == board_list.name:
                    return board_list


if __name__ == "__main__":
    warnings.filterwarnings("ignore")
    print "####### Getting notes from evernote..."
    evernote_notes = fetch_evernote_notes()
    print "{} notes fetched from evernote".format(len(evernote_notes))

    print "\n####### Fetching trello list to create new cards"
    trello_list = get_trello_list()

    print "\n####### Creating cards from notes..."
    counter = 1
    for title, content in evernote_notes.iteritems():
        content = html2text.html2text(content.decode('utf-8'))
        card = trello_list.add_card(title, desc=content)
        print "Card created: [{}] {}".format(counter, title)
        counter += 1

    print "\n####### Phew! Bye."
```


## References

1. [The Evernote SDK for Python Quick-start Guide](https://dev.evernote.com/doc/start/python.php)
2. [evernote/evernote-sdk-python](https://github.com/evernote/evernote-sdk-python/blob/master/sample/client/EDAMTest.py)
3. [Read My Own Evernotes](http://stackoverflow.com/questions/20080414/read-my-own-evernotes)
4. [Search Grammar](https://dev.evernote.com/doc/articles/search_grammar.php)
5. [`findNotesMetadata`](https://dev.evernote.com/doc/reference/NoteStore.html#Fn_NoteStore_findNotesMetadata)
6. [Trello API Documentation](https://trello.com/docs/)
7. [sarumont/py-trello](https://github.com/sarumont/py-trello)
8. [Trello - Developer API Keys](https://trello.com/1/appKey/generate)
9. [Getting your Trello OAuth Token](https://github.com/sarumont/py-trello#getting-your-trello-oauth-token)
10. [aaronsw/html2text](https://github.com/aaronsw/html2text)