$member_id = $request->employee_id;
        $query = Member::where('member_id', $member_id);

        $query->join('designations', 'members.designation_id', '=', 'designations.designation_id')
        ->join('departments', 'members.department_id', '=', 'departments.department_id')
        ->join('office_branches', 'members.branch_id', '=', 'office_branches.branch_id')
        ->leftJoin('bank_accounts', function ($join) {
            $join->on('members.member_id', '=', 'bank_accounts.employee_id')
                ->where('bank_accounts.is_deleted', 0)
                ->where('bank_accounts.is_active', 1);
        })
        ->select(
            'members.*',
            'designations.designation_name',
            'departments.department_name',
            'office_branches.branch_name',
            'bank_accounts.account_id',
            'bank_accounts.account_type',
            'bank_accounts.bank_name',
            'bank_accounts.account_name',
            'bank_accounts.account_no',
            'bank_accounts.short_code',
            'bank_accounts.routing_no'
        );

 $query = $query->first();
