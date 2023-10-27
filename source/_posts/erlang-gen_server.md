---
title: process_flag(trap_exit, true)
date: 2023-05-05 11:02:51
tags: erlang
categories: erlang
description: ""
---

# [对 erlang 中 process_flag(trap_exit, true)的解释](https://stackoverflow.com/questions/6720472/erlang-and-process-flagtrap-exit-true)

文档解释：

> ### process_flag(Flag :: trap_exit, Boolean) -> `OldBoolean`
>
> > When `trap_exit` is set to `true`, exit signals arriving to a process are converted to `{'EXIT', From, Reason}` messages, which can be received as ordinary messages. If `trap_exit` is set to `false`, the process exits if it receives an exit signal other than `normal` and the exit signal is propagated to its linked processes. Application processes are normally not to trap exits.
> >
> > Returns the old value of the flag.

1. 如果不关心子进程的状态

   ```erlang
   spwan(...)
   ```

2. 如果子进程崩溃，我也随之崩溃

   ```erlang
   spawn_link(...)
   ```

3. 如果子进程正常终止 或 非正常终止，我想要捕获退出的消息

   ```erlang
   process_flag(trap_exit, true),
   spawn_link(...)
   ```

Please see this example and try different values (inverse with 2 or 0 to provoke an exception, and using trap_exit or not):

```erlang
-module(play).
-compile(export_all).

start() ->
    process_flag(trap_exit, true),
    spawn_link(?MODULE, inverse, [2]),
    loop().

loop() ->
    receive
        Msg -> io:format("~p~n", [Msg])
    end,
    loop().

inverse(N) -> 1/N.
```

```erlang
{Info, CommentL} =
    case lists:keyfind(?E_g_statue_type_power, #s_statue_info.type, AllInfo) of
      false ->
        {?DB_INSERT_RECORD_INC(s_statue_info, #s_statue_info{type = ?E_g_statue_type_pvp, update_ts = TS}), []};
      OldInfo ->
        OldCommentL = ?DB_SELECT_RECORDS(s_statue_comment, "statue_info", OldInfo#s_statue_info.guid),
        case is_update(OldInfo) of
          true ->
            case get_new_power_statue_info() of
              undefined ->
                NewInfo = OldInfo#s_statue_info{update_ts = TS},
                ?DB_UPDATE_RECORD(s_statue_info, OldInfo, NewInfo),
                {NewInfo, OldCommentL};

              StatueInfo when StatueInfo#s_statue_info.player_id == OldInfo#s_statue_info.player_id ->
                {OldInfo, OldCommentL};

              StatueInfo ->
                ?DB_DELETE_RECORDS(s_statue_comment, "statue_info", OldInfo#s_statue_info.guid),
                ?DB_DELETE_RECORD(s_statue_info, OldInfo),
                {StatueInfo, []}
            end;
          false ->
            {OldInfo, OldCommentL}
        end,
        ok
    end,
  PowerStatue = #statue{type = ?E_g_statue_type_power, player_id = Info#s_statue_info.player_id, statue_info = Info, comment_list = CommentL},
  ArenaStatue =
                end,
```
