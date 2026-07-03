이벤트 목록
-----------------

아래의 목록은 라이믹스 프레임워크 및 코어에 포함된 모듈에서 공식적으로 제공하는 이벤트입니다.
그 밖에도 다른 모듈에서 임의로 이벤트를 제공할 수 있습니다.

대부분의 이벤트는 `before`와 `after`의 한 쌍을 이루어 두 차례씩 발생합니다.
`before` 이벤트는 어떤 작업이 일어나기 전에 발생하고, `after` 이벤트는 작업 완료 후에 발생합니다.

원칙적으로, `before` 시점에는 `BaseObject(-1, '에러메시지')`를 반환하거나 `Rhymix\Framework\Exception`을 던져서
해당 작업을 중단시킬 수 있습니다. 예를 들어 댓글이 작성되지 않도록 하거나, 파일이 다운로드되는 것을 막을 수 있습니다.
반면, `after` 시점에 반환한 오브젝트나 던진 예외는 일반적으로 무시됩니다.
이미 일어난 작업에 대해 통보를 받는 것에 불과하기 때문입니다.

### 코어 라이프사이클

| 이벤트 이름          | 호출 시점      | 비고 |
|--------------------|--------------|------|
| moduleHandler.init | before/after | |
| moduleHandler.proc | after        | |
| layout             | before       | |
| display            | before/after | |

### 모듈 라이프사이클

| 모듈    | 이벤트 이름          | 호출 시점      | 비고 |
|--------|--------------------|--------------|------|
| module | module.deleteModule | before/after | |
| module | module.dispAdditionSetup | before/after | |
| module | module.dispAdditionSetup | before/after | |
| module | module.getModuleAdminScopes | after | |
| module | module.insertModuleConfig | before/after | 2.2+ |
| module | module.insertModulePartConfig | before/after | 2.2+ |
| module | module.procModuleAdminCopyModule | after | |

### 회원

| 모듈    | 이벤트 이름          | 호출 시점      | 비고 |
|--------|--------------------|--------------|------|
| member | member.addMemberToGroup | before/after | |
| member | member.deleteGroup | before/after | |
| member | member.deleteMember | before/after | |
| member | member.deleteMember | before/after | |
| member | member.deleteScrapDocument | before/after | |
| member | member.dispMemberSignUpForm | before | |
| member | member.doAutoLogin | before/after | |
| member | member.doLogin | before/after | |
| member | member.doLogout | before/after | |
| member | member.getMemberMenu | before/after | |
| member | member.insertGroup | before/after | |
| member | member.insertMember | before/after | |
| member | member.insertMemberDevice | before/after | |
| member | member.procMemberAuthAccount | before/after | |
| member | member.procMemberCheckValue | before/after | |
| member | member.procMemberInsert | before/after | |
| member | member.procMemberModifyInfo | before/after | |
| member | member.procMemberScrapDocument | before/after | |
| member | member.removeMemberFromGroup | before/after | |
| member | member.updateGroup | before/after | |
| member | member.updateMember | before/after | |
| member | member.updateMemberEmailAddress | after | |

### 문서

| 모듈    | 이벤트 이름          | 호출 시점      | 비고 |
|--------|--------------------|--------------|------|
| document | document.copyDocumentModule | add | deprecated |
| document | document.copyDocumentModule | before/after | |
| document | document.copyDocumentModule.each | before/after | |
| document | document.declaredDocument | before/after | |
| document | document.declaredDocumentCancel | before/after | |
| document | document.deleteCategory | before/after | |
| document | document.deleteDocument | before/after | |
| document | document.getComments | after | |
| document | document.getDocumentList | before/after | |
| document | document.getDocumentMenu | before/after | |
| document | document.getNoticeList | before/after | |
| document | document.getThumbnail | before | |
| document | document.insertCategory | before/after | |
| document | document.insertDocument | before/after | |
| document | document.manage | before/after | |
| document | document.moveDocumentModule | before/after | |
| document | document.moveDocumentToTrash | before/after | |
| document | document.publishDocument | before/after | |
| document | document.restoreTrash | after | |
| document | document.updateCategory | before/after | |
| document | document.updateDocument | before/after | |
| document | document.updateReadedCount | before/after | |
| document | document.updateVotedCount | before/after | |
| document | document.updateVotedCountCancel | before/after | |

### 댓글

| 모듈    | 이벤트 이름          | 호출 시점      | 비고 |
|--------|--------------------|--------------|------|
| comment | comment.copyCommentByDocument | add | deprecated |
| comment | comment.copyCommentByDocument.each | before/after | |
| comment | comment.declaredComment | before/after | |
| comment | comment.declaredCommentCancel | before/after | |
| comment | comment.deleteComment | before/after | |
| comment | comment.getCommentList | before/after | |
| comment | comment.getCommentMenu | before/after | |
| comment | comment.getThumbnail | before | |
| comment | comment.getTotalCommentList | before/after | |
| comment | comment.insertComment | before/after | |
| comment | comment.moveCommentToTrash | before/after | |
| comment | comment.procCommentAdminChangeStatus | after | |
| comment | comment.sendEmailToAdminAfterInsertComment | after | |
| comment | comment.updateComment | before/after | |
| comment | comment.updateVotedCount | before/after | |
| comment | comment.updateVotedCountCancel | before/after | |

### 파일

| 모듈    | 이벤트 이름          | 호출 시점      | 비고 |
|--------|--------------------|--------------|------|
| file | file.deleteFile | before/after | |
| file | file.downloadFile | before/after | |
| file | file.insertFile | before/after | |

### 커뮤니케이션 모듈

| 모듈    | 이벤트 이름          | 호출 시점      | 비고 |
|--------|--------------------|--------------|------|
| communication | communication.addFriend | before/after | |
| communication | communication.deleteFriend | before/after | |
| communication | communication.deleteMessage | before/after | |
| communication | communication.deleteMessages | before/after | |
| communication | communication.sendMessage | before/after | |

### 회원 포인트

| 모듈    | 이벤트 이름          | 호출 시점      | 비고 |
|--------|--------------------|--------------|------|
| point  | point.setPoint     | before/after | |

### 기타

| 모듈    | 이벤트 이름          | 호출 시점      | 비고 |
|--------|--------------------|--------------|------|
| admin | admin.dashboard | before | |
| board | module.dispAdditionSetup | before/after | |
| editor | editor.deleteSavedDoc | after | |
| install | menu.getModuleListInSitemap | after | |
| menu | menu.getModuleListInSitemap | after | |
| ncenterlite | ncenterlite._insertNotify | before/after | |
