# C

select c.contr_id,

       c.contr_no,

       ch.contr_hold_instr_id,

       ch.hold_reference,

       ch.hold_reason

  from promis_Dba.Container              c,

       promis_Dba.Contr_Cycle            cc,

       promis_Dba.Contr_Hold_Boxes       cb,

       promis_Dba.Contr_Hold_Instruction ch

where cc.contr_id = c.contr_id

   and c.is_active = 1

   and c.oprn_status = 1

   and cc.is_active = 1

   and cc.oprn_status = 1

      --and c.contr_no = 'CMAU473252'

      --'MRKU682825'--= 'WHSU568388'

   and cc.contr_id = cb.contr_id

   and cc.contr_cycle_id = cb.contr_cycle_id

   and cb.contr_hold_instr_id = ch.contr_hold_instr_id

   and ch.is_active = 1

   and sysdate between ch.valid_from and ch.valid_to

   and ch.hold_type_id in (select cc.code_id

                             from promis_Dba.Config_Codes cc

                            where cc.code_type = 'STOP_TYPES'

                              and cc.short_code = '94'

                              and cc.is_active = 1)

   and not exists

(select 1

          from promis_Dba.Contr_Hold_Release chr

         where chr.contr_hold_instr_id = ch.contr_hold_instr_id);
