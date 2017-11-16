## Example

```ocaml
let options =
   {
	"apiKey": "...",
	"authDomain": "...",
	"databaseURL": "...",
	"storageBucket": "...",
	"messagingSenderId": "..."
   };

let app = ReasonFirebase.initializeApp(options);

let db = ReasonFirebase.App.database(app);

ReasonFirebase.Database.Reference.once(
      ReasonFirebase.Database.ref(db, ~path="ticket", ()),
      ~eventType="value",
      ()
    )
    |> Js.Promise.then_(
      (teamDomain) => ReasonFirebase.Database.DataSnapshot.val_(teamDomain)
        |> (ticket) => parseTicket(ticket)
        |> (ticketJson) => Js.log(ticketJson) //here you got a record of ticket type
        |> Js.Promise.resolve
    );

type ticket = {
  id: int,
  name: string
};

type state =  {
  ticket : ticket
};

let parseTicket = (json): ticket =>
  Json.Decode.{ //Json.Decode is from module https://github.com/reasonml-community/bs-json
      id: json |> field("id",int),
      name: json |> field("name", string)
  };
```
