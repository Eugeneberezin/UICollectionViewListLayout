# UICollectionViewListLayout
A sample code for UICollectionLayoutListConfiguration.  TableView like looking cell i Collection View.  

Step by step instructions: 

Create Section: 
    enum Section {
    
        case main
        
    }
    
    
Create Layout: 

    private func createLayout() -> UICollectionViewCompositionalLayout {
        let config = UICollectionLayoutListConfiguration(appearance: .plain)
        return UICollectionViewCompositionalLayout.list(using: config)
    }
    
 
Configure collection view: 

    private func configureCollectionView() {
        collectionView = UICollectionView(frame: view.bounds, collectionViewLayout: createLayout())
        view.addSubview(collectionView)
        collectionView.delegate = self
    }
    
    
Configure dataSource: 

    private func dataSourceConfig() {
        let cellRegistration = UICollectionView.CellRegistration<UICollectionViewListCell, Int> {(cell, indexPath, item) in
            
            var content = cell.defaultContentConfiguration()
            content.image = UIImage(systemName: "flame")
            content.text = "Text for cell \(item)"
            cell.contentConfiguration = content
        }
        dataSource = UICollectionViewDiffableDataSource<Section, Int>(collectionView: collectionView) {(collectionView: UICollectionView, indexPath: IndexPath, identifier: Int) in
            return collectionView.dequeueConfiguredReusableCell(using: cellRegistration, for: indexPath, item: identifier)
        }
        
        var snapshot = NSDiffableDataSourceSnapshot<Section, Int>()
        snapshot.appendSections([.main])
        snapshot.appendItems(Array(1...10))
        dataSource.apply(snapshot, animatingDifferences: true)
    }
    
