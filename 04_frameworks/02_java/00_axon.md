[TOC]

# I Getting started



## introduction

command query responsibility segregation通过彻底改变应用架构解决应用的扩展性和伸缩性。与将不同逻辑放入不同层不同，逻辑分离是依据是否改变或查询应用状态。查询与修改分离。当命令执行以后，查询同步或异步的通过事件更新。这种通过事件更新的机制，使得这种架构更易于扩展，伸缩以及最后更易于维护

CQRS与层架构不同，

### disruptor

#### ringbuffer

#### sequence

#### sequencer



## archetecture overview

## messaging concepts

## configuration api

# II domain logic

## command model

## event handing

## sagas

## Testing

# III infrastructure components

## command dispatching

## event processing

## repository and event store

## spring boot autoconfiguration

# IV advanced tuning

## advanced customizations

## performance tuning



