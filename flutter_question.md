Ho un quesito e siccome ne sto uscendo pazzo chiedo aiuto a voi.

Ho una UI che consiste in un pulsante e una listview. Alla pressione del pulsante viene chiamato un metodo del viewmodel.

`viewModel.getData();`

il quale di fatto non fa altro che invocare un metodo del repository

`Repository.getBeers()`

che è asincrono 

```
static Future<List<Beer>> getBeers() async {

    final beers = await Network.getBeerData();

    final List beerJsonList = jsonDecode(beers);
    final List<Beer> result = List();

    beerJsonList.forEach((jsonObject) {
      final beer = _withJson(jsonObject);
      result.add(beer);
    });

    return result;
}
```

e che non fa altro che invocare un endpoint

```
class Network {
  static final url = "https://api.punkapi.com/v2/beers?page=1&per_page=30";

  static Future<String> getBeerData() {
    return http.get(url).then((value) => value.body);
  }
}
```

fin qui tutto bene. La mia domanda e' perchè nel view model 

```
Repository.getBeers().then((value) {
      viewState = ViewState.completed;
      this.data = value;
      notifyListeners();
    });
```

devo utilizzare il then ? E' corretto? 
Mi aspettavo di poter utilizzare l'await. 

Grazie a tutti quelli che mi risponderanno