# Dune mkdir crash repro

To reproduce, build dune from source and put the executable in your PATH (named `dune`).
Then run:

```
dune clean && dune build
```

The crash is non-deterministic, happening about 1 in 3 times on my machine (macos arm64):

```
Internal error, please report upstream including the contents of _build/log.
Description:
  ("failed to create parent directory",
  { t_s =
      "_build/.sandbox/8cc1e2f4b23197a3c96d399a8e8423d5/_private/default/.pkg"
  })
Raised at Stdune__Code_error.raise in file
  "otherlibs/stdune/src/code_error.ml", line 10, characters 30-62
Called from Stdune__Fpath.mkdir_p in file "otherlibs/stdune/src/fpath.ml",
  line 35, characters 12-33
Called from Stdune__Path.Relative_to_source_root.mkdir_p in file
  "otherlibs/stdune/src/path.ml" (inlined), line 508, characters 12-55
Called from Stdune__Path.Outside_build_dir.mkdir_p in file
  "otherlibs/stdune/src/path.ml", line 567, characters 25-65
Called from Dune_engine__Sandbox.create_dirs in file
  "src/dune_engine/sandbox.ml", line 111, characters 2-23
Called from Dune_engine__Sandbox.create.(fun) in file
  "src/dune_engine/sandbox.ml", line 201, characters 6-35
Called from Stdune__Exn_with_backtrace.try_with in file
  "otherlibs/stdune/src/exn_with_backtrace.ml", line 9, characters 8-12
Re-raised at Stdune__Exn.raise_with_backtrace in file
  "otherlibs/stdune/src/exn.ml", line 38, characters 27-56
Called from Fiber__Core.O.(>>|).(fun) in file "vendor/fiber/src/core.ml",
  line 253, characters 36-41
Called from Fiber__Scheduler.exec in file "vendor/fiber/src/scheduler.ml",
  line 76, characters 8-11
-> required by
   ("execute-rule",
   { id = 9
   ; info =
       From_dune_file
         { pos_fname = "<none>"
         ; start = { pos_lnum = 1; pos_bol = 0; pos_cnum = 0 }
         ; stop = { pos_lnum = 1; pos_bol = 0; pos_cnum = 0 }
         }
   })
-> required by ("<unnamed>", ())
-> required by
   ("build-file",
   In_build_dir "_private/default/.pkg/stdlib-shims/target/cookie")
-> required by ("pkg-resolve", <opaque>)
-> required by ("pkg-resolve", <opaque>)
-> required by ("native-context", ())
-> required by ("build-contexts", ())
-> required by ("toplevel", ())

I must not crash.  Uncertainty is the mind-killer. Exceptions are the
little-death that brings total obliteration.  I will fully express my cases.
Execution will pass over me and through me.  And when it has gone past, I
will unwind the stack along its path.  Where the cases are handled there will
be nothing.  Only I will remain.
```
