# laravel-tips
## Minimize, remove Elequent query  with laravel scope
### BEFORE
```

 public function getAllComments() {
        return $this
            ->join('users', 'users.user_id', '=', 'comments.user_id')
            ->where('status', 'active')
            ->get();
    }

    public function getRecentComments() {
        return $this
            ->join('users', 'users.user_id', '=', 'comments.user_id')
            ->where('status', 'active')
            ->orderBy('comments.created_at', 'desc')
            ->limit(100)
            ->get();
    }
    
     public function getInactiveUserComments() {
        return $this
            ->join('users', 'users.user_id', '=', 'comments.user_id')
            ->where('status', 'inactive')
            ->get();
    }
 ```
 ### AFTER
```    
    public function getAllComments() {
        return $this
            ->CommonUserAndComment('active')
            ->get();
    }
    public function getRecentComments() {
        return $this
            ->CommonUserAndComment('active')
            ->orderBy('comments.created_at', 'desc')
            ->limit(100)
            ->get();
    }
    public function getInactiveUserComments() {
        return $this
            ->CommonUserAndComment('inactive')
            ->get();
    }
    public function scopeCommonUserAndComment($scope, $status) {
        return $scope
            ->UserJoin()
            ->UserStatus($status);
    }
    public function scopeUserJoin($scope) {
        return $scope->join('users', 'users.user_id', '=', 'comments.user_id');
    }
    public function scopeUserStatus($scope, $status) {
        return $scope->where('status', $status);
    }
    
```
