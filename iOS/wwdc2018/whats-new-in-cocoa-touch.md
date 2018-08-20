# What's New in Cocoa Touch



## Framework updates

### Performance



#### Scrolling

```swift
// UITableView Cell Load Cost

class MyTableViewDataSource {
    func tableView(_ tableView: UITableView,
        cellForRowAt indexPath: IndexPath) -> UITableViewCell {
        // Dequeue or allocate cell
        // Populate cell with model data
        
        return cell
    }
}

// call layoutSubviews on UIViews in the cell
// call draw() on UIViews in the cell
```

```swift
// UITableView Pre-Fetching

protocol UITableViewDataSourcePrefetching {
    func tableView(_ tableView: UITableView, prefetchRowsAt indexPaths: [IndexPath])
    func tableView(_ tableView: UITableView, cancelPrefetchingForRowsAt indexPaths: [IndexPath])
}
```



#### Memory

##### Automatic Backing Store

Automatic is now default

* `UIView.draw()`

* `UIGraphicsImageRenderer`

* `UIGraphicsImageRendererFormat.Range`

  

#### Auto Layout





## API enhancements



## Siri shortcuts




