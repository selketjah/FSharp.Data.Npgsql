## Description
FSharp.Data.Npgsql is F# type providers library on a top of well-known [Npgsql ADO.NET client library]( http://www.npgsql.org/doc/index.html). 

The library includes two type providers: NpgsqlConnection and NpgsqlCommand. It's recommended to use NpgsqlConnection by default.  NpgsqlCommand exists mainly for flexibility.

## Target platforms: 
  - netstandard2.0
  - net461

## Getting started

All examples based on [DVD rental sample database](http://www.postgresqltutorial.com/download/dvd-rental-sample-database/) and assume following definitions to exist.
```fsharp
[<Literal>]
let dvdRental = "Host=localhost;Username=postgres;Database=dvdrental;Port=32768"

open FSharp.Data.Npgsql

type DvdRental = NpgsqlConnection<dvdRental>
```

### Basic query to get data

```fsharp
use cmd = DvdRental.CreateCommand<"SELECT title, release_year FROM public.film LIMIT 3">(dvdRental)

for x in cmd.Execute() do   
    printfn "Movie '%s' released in %i." x.title x.release_year.Value
```
Alternatevly using inline ```NpgsqlCommand``` definition.
```fsharp
use cmd = new NpgsqlCommand<"SELECT title, release_year FROM public.film LIMIT 3", dvdRental>(dvdRental)
//...
```
Or using ```NpgsqlCommand``` with explicit type alias. `Create` factory method can be used in addition to traditional constructor. It mainly exists to work around [Intellisense deficiency]().

```fsharp
type BasicQuery = NpgsqlCommand<"SELECT title, release_year FROM public.film LIMIT 3", dvdRental>
use cmd = BasicQuery.Create(dvdRental)
//...
```

## Configuration

## Scripting

## Data modifications

## Transactions

## Limitations

  #### One unfortunate PostgreSQL limitation is that column nullability cannot be inferred for derived columns. A command 
  ```fsharp
  use cmd = new NpgsqlCommand<"SELECT 42 AS Answer", dvdRental>(dvdRental)
  assert( cmd.Execute() |> Seq.exactlyOne = Some 42)
  ```

## Running tests

## Implemenation details
