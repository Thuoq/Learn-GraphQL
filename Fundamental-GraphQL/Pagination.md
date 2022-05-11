## Graph Pagination

Đây là cái nơi chúng ra return concept của **pagination** ở đây là phân trang. Hãy tưởng tượng rằng bạn có 1 danh sách các repo in your Github, nhưng bạn chỉ muốn retrieve 1 vài trong số chúng để hiển thị trong UI. Trong GraphQL, bạn có thể **request paginated** data bởi cung cấp arguments to 1 danh dách fied

```
query OrganizationForLearningReact {
	organization(login:"the-road-to-learn-react") {
  	... FiledAddOrganization
    repositories(first:2) {
      edges {
      	node {
        	name
        }
      }
    }
  }

}
fragment FiledAddOrganization on Organization {
  url
  name
}
```

Chúng ta sẽ giải thích cái câu lệnh ở trên, chúng ta pass một **first** argument được passed trong **repositories** list field rằng chỉ định có bao nhiêu items từ lust chúng ta expected in the result. The query

```
# Type queries into this side of the screen, and you will
# see intelligent typeaheads aware of the current GraphQL type schema,
# live syntax, and validation errors highlighted within the text.

# We'll get you started with a simple query showing your username!
# query {
#   organization(login:"the-road-to-learn-react") {
#     name
#     url
#     repository(name:"the-road-to-learn-react") {
#       id
#       name

#     }
#   }

# }
# mutation AddStart($repositoryId:ID!) {
#   addStar(input:{starrableId:$repositoryId}) {
#   	starrable {
#     	id
#       viewerHasStarred
#     }
#   }
# }


query OrganizationForLearningReact {
	organization(login:"the-road-to-learn-react") {
  	... FiledAddOrganization
    repositories(first:2) {
      edges {
      	node {
        	name
        }
        cursor
      }
    }
  }

}
fragment FiledAddOrganization on Organization {
  url
  name
}
```

The **totalCount** field discolose the total number of items in the list, while the **pageInfo** field gives you information about two things:

1. **EndCursor**:có thể được sử dụng để truy xuất danh sách kế tiếp, mà chúng tôi đã làm với trường con trỏ, ngoại trừ lần này chúng tôi chỉ cần một trường meta để thực hiện nó. Con trỏ của mục danh sách cuối cùng là đủ để yêu cầu trang tiếp theo của danh sách

2. **hasNextPage**:cung cấp cho bạn thông tin về việc có hay không trang tiếp theo để truy xuất từ API GraphQL. Đôi khi bạn đã tìm nạp trang cuối cùng từ máy chủ của mình. Đối với các ứng dụng sử dụng tính năng cuộn vô hạn để tải thêm trang khi cuộn danh sách, bạn có thể ngừng tìm nạp các trang khi không còn trang nào nữa.
