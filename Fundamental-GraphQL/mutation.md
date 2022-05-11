## GraphQL Operation: Mutation

This section giới thiệu the GraphQL mutation. Nó hoàng toàn the GraphQL query bởi vì nó thường được sử dụng để viết data thay vì đọc dữ liêu. The mutation sẽ chia sẻ the same principles như 1 query. Nó có các **fields** và các **objects**, **arguments** và **variable**, **fragment** và **operation** name, cũng như là **directives**

Đầu tiên chúng ta phải xem id của cái repository đó có ID là gì để chúng ta thêm id vào

```
query {
  organization(login:"the-road-to-learn-react") {
    name
    url
    repository(name:"the-road-to-learn-react") {
      id
      name

    }
  }
query {
  organization(login:"the-road-to-learn-react") {
    name
    url
    repository(name:"the-road-to-learn-react") {
      id
      name

    }
  }
}
```

**Mutation**

```
mutation AddStart($repositoryId:ID!) {
  addStar(input:{starrableId:$repositoryId}) {
  	starable {
    	id
      viewerHasStarred
    }
  }
}
```

Mutation name này được cung cấp bởi GitHub's API addStart, Bạn có thể yêu cầi để passis the `starrableId` như 1 input để identify the repositioty, không thì GitHub Server sẽ không biết which Repository nào start với mutation. Thêm nữa , mutation là 1 tên đã được đặt là AddStar. Last by not least, bạn có thể định nghĩa giá trị trả về của mutation bằng cách sử dụng objects và fields again.
