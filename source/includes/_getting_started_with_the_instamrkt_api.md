# Getting started with the Instamrkt API (1.0)

## Overview
### Introduction
Instamrkt is an API based prediction market with 2 modes of operation. Both the REST version of the API and the websocket mode support similary functionality but with different operational characteristics. Websockets are push based while REST must be polled to retrieve updates. 

Regardless of the connection mechanism much of the terminology is the same so let's start with that. 

### How we view the world (terminology)
Every registerd site or app has a unique *source* defined by a **source_id** that identifies the owner of the games underneath it. It serves as a top level grouping for all games that happen underneath it. 

A *game* defined by a **game_id** is a single game (full game, not set or half or quarter or period) - e.g. Chelsea v. Man United, Federer v. Nadal, Knicks v. Nets. 