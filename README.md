# laravel-tips
## Minimize, remove repeted Elequent query  with laravel scope
### BEFORE
```

   public function getAllComments() {
        return $this
            ->join('users', 'users.user_id', '=', 'comments.user_id')
            ->join('user_roles', 'users.user_id', '=', 'user_roles.user_id')
            ->join('roles', 'user_roles.role_id', '=', 'roles.role_id')
            ->where('roles.name', 'admin')
            ->where('status', 'active')
            ->get();
    }

    public function getRecentComments() {
        return $this
            ->join('users', 'users.user_id', '=', 'comments.user_id')
            ->join('user_roles', 'users.user_id', '=', 'user_roles.user_id')
            ->join('roles', 'user_roles.role_id', '=', 'roles.role_id')
            ->where('roles.name', 'admin')
            ->where('status', 'active')
            ->orderBy('comments.created_at', 'desc')
            ->limit(100)
            ->get();
    }

    public function getInactiveUserComments() {
        return $this
            ->join('users', 'users.user_id', '=', 'comments.user_id')
            ->join('user_roles', 'users.user_id', '=', 'user_roles.user_id')
            ->join('roles', 'user_roles.role_id', '=', 'roles.role_id')
            ->where('roles.name', 'admin')
            ->where('status', 'inactive')
            ->get();
    }

    public function getUserComments() {
        return $this
            ->join('users', 'users.user_id', '=', 'comments.user_id')
            ->join('user_roles', 'users.user_id', '=', 'user_roles.user_id')
            ->join('roles', 'user_roles.role_id', '=', 'roles.role_id')
            ->where('roles.name', 'user')
            ->where('status', 'active')
            ->get();
    }

    public function getInactiveUserRoleComments() {
        return $this
            ->join('users', 'users.user_id', '=', 'comments.user_id')
            ->join('user_roles', 'users.user_id', '=', 'user_roles.user_id')
            ->join('roles', 'user_roles.role_id', '=', 'roles.role_id')
            ->where('roles.name', 'user')
            ->where('status', 'inactive')
            ->get();
    }

 ```
 ### AFTER
```    
     public function getAllComments() {
        return $this
            ->CommonUserAndComment('active', 'admin')
            ->get();
    }
    public function getRecentComments() {
        return $this
            ->CommonUserAndComment('active', 'admin')
            ->orderBy('comments.created_at', 'desc')
            ->limit(100)
            ->get();
    }
    public function getInactiveUserComments() {
        return $this
            ->CommonUserAndComment('inactive', 'admin')
            ->get();
    }
    public function getUserComments() {
        return $this
            ->CommonUserAndComment('active', 'user')
            ->get();
    }
    public function getInactiveUserRoleComments() {
        return $this
            ->CommonUserAndComment('inactive', 'user')
            ->get();
    }
    public function scopeCommonUserAndComment($scope, $status, $role) {
        return $scope
            ->UserJoin()
            ->UserRoleJoin()
            ->RoleJoin()
            ->UserStatus($status)
            ->UserRole($role);
    }
    public function scopeUserJoin($scope) {
        return $scope->join('users', 'users.user_id', '=', 'comments.user_id');
    }
    public function scopeUserRoleJoin($scope) {
        return $scope->join('user_roles', 'users.user_id', '=', 'user_roles.user_id');
    }
    public function scopeRoleJoin($scope) {
        return $scope-->join('roles', 'user_roles.role_id', '=', 'roles.role_id');
    }
    public function scopeUserStatus($scope, $status) {
        return $scope->where('status', $status);
    }
    public function scopeUserRole($scope, $role) {
        return $scope->where('roles.name', $role);
    }
    
```
